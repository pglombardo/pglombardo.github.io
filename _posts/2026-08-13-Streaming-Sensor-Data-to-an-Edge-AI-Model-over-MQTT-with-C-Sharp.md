---
layout: single
categories: ["C#", "HiveMQ"]
title: "Streaming Sensor Data to an Edge AI Model over MQTT with C#"
canonical_url: "https://www.hivemq.com/blog/streaming-sensor-data-to-edge-ai-mqtt/"
header:
  teaser: /assets/images/posts/2026/streaming-sensor-data-edge-ai.jpg
---

_This post originally appeared on the [HiveMQ blog](https://www.hivemq.com/blog/streaming-sensor-data-to-edge-ai-mqtt/)._

Edge AI needs a data path before it needs a model. Sensors produce readings on the device or the local network, the model runs nearby, and something has to move the readings from the first to the second — quickly, and without losing them. MQTT is the natural carrier for that traffic, and on .NET the [HiveMQ MQTT client](https://github.com/hivemq/hivemq-mqtt-client-dotnet) makes the transport part straightforward. The part that takes real thought is feeding the model at the right rate: fast enough to keep up, without dropping readings or exhausting memory when the model falls behind. This post builds that ingest path and shows how to let the model's own throughput set the pace.

_Tested against HiveMQtt v0.45.0 (July 2026)._

## Where naive ingest breaks

A sensor stream and an inference loop run at different speeds, and that gap is the whole problem. Run the model directly inside the message handler and each reading blocks the next — the client processes one at a time and the backlog piles up behind you. Spin up a task per message instead and a burst spawns thousands of them, so the process runs out of memory before the model catches up. Neither survives contact with a real sensor.

What you want is bounded intake with back-pressure: a fixed ceiling on how much work is in flight, and a way to say "slow down" that reaches back toward the source. MQTT 5 gives you exactly that, and the client exposes it directly.

## Let the model set the pace

The mechanism is [flow control](https://docs.oasis-open.org/mqtt/mqtt/v5.0/mqtt-v5.0.html#_Toc3901251). A client advertises a [Receive Maximum](https://docs.oasis-open.org/mqtt/mqtt/v5.0/mqtt-v5.0.html#_Toc3901049) — the number of QoS 1 and 2 messages it will accept before it has acknowledged the ones it already has. Pair that with [manual acknowledgement](https://hivemq.github.io/hivemq-mqtt-client-dotnet/docs/hivemqtt/how-to/manual-ack) and you get a throttle wired straight to your model: don't acknowledge a reading until the model has consumed it, and the broker will stop sending once the in-flight count hits the ceiling.

```c#
using HiveMQtt.Client;
using HiveMQtt.Client.Options;
using HiveMQtt.MQTT5.Types;

var options = new HiveMQClientOptionsBuilder()
    .WithBroker("broker.hivemq.com")
    .WithClientId("edge-inference-01")
    .WithManualAck()          // we acknowledge only after the model has the reading
    .WithReceiveMaximum(8)    // at most 8 unacknowledged QoS 1 messages in flight
    .Build();

var client = new HiveMQClient(options);

client.OnMessageReceived += async (sender, args) =>
{
    var payload = args.PublishMessage.Payload ?? Array.Empty<byte>();

    var reading = SensorReading.Decode(payload);          // your deserialization
    await model.InferAsync(reading).ConfigureAwait(false); // your edge model

    // Only now does the broker count this message as handled and send the next.
    await client.AckAsync(args).ConfigureAwait(false);
};

await client.ConnectAsync().ConfigureAwait(false);
await client.SubscribeAsync("sensors/+/telemetry", QualityOfService.AtLeastOnceDelivery)
            .ConfigureAwait(false);
```

`SensorReading.Decode` and `model.InferAsync` are your code — the decoder for your payload format and the call into your model. Everything around them is the client doing flow control for you. Because the acknowledgement is deferred until inference finishes, a slow model simply means fewer acknowledgements per second, which means the broker sends fewer messages per second. The pressure propagates backward on its own.

One behavior worth knowing: since v0.44.0 the client serializes QoS 1 and 2 `OnMessageReceived` handlers in [FIFO order](https://github.com/hivemq/hivemq-mqtt-client-dotnet/releases/tag/v0.44.0), so readings reach your model one at a time, in the order they arrived. For a model that processes sequentially, that ordering is exactly what you want.

## Route different sensors to different handlers

A real device rarely publishes one kind of reading. When temperature and vibration need different preprocessing or different models, attach a handler per subscription instead of branching on the topic inside one big callback:

```c#
var subscribeOptions = new SubscribeOptionsBuilder()
    .WithSubscription("sensors/+/temperature", QualityOfService.AtLeastOnceDelivery,
        messageReceivedHandler: OnTemperature)
    .WithSubscription("sensors/+/vibration", QualityOfService.AtLeastOnceDelivery,
        messageReceivedHandler: OnVibration)
    .Build();

await client.SubscribeAsync(subscribeOptions).ConfigureAwait(false);
```

Each handler is an `EventHandler<OnMessageReceivedEventArgs>`, so the same manual-acknowledgement pattern applies inside each one. The [wildcard](https://hivemq.github.io/hivemq-mqtt-client-dotnet/docs/hivemqtt/subscribing) `+` matches any single level, so one subscription covers every device publishing that reading.

## When you can afford to drop: QoS 0 and a bounded buffer

Flow control only exists for QoS 1 and 2. A high-rate telemetry stream where the latest value matters more than every value is often better served by [QoS 0](https://docs.oasis-open.org/mqtt/mqtt/v5.0/mqtt-v5.0.html#_Toc3901234) — at-most-once, no acknowledgements, no broker-side queue. There's no protocol back-pressure to lean on, so bound the intake yourself and decide, explicitly, what to drop under load. A bounded channel does this cleanly:

```c#
using System.Threading.Channels;

var channel = Channel.CreateBounded<byte[]>(new BoundedChannelOptions(1000)
{
    FullMode = BoundedChannelFullMode.DropOldest, // shed the stalest reading, never block
});

client.OnMessageReceived += (sender, args) =>
{
    channel.Writer.TryWrite(args.PublishMessage.Payload ?? Array.Empty<byte>());
};

// A single consumer runs inference at whatever rate the model sustains.
await foreach (var payload in channel.Reader.ReadAllAsync())
{
    await model.InferAsync(SensorReading.Decode(payload)).ConfigureAwait(false);
}
```

With `DropOldest`, a burst that outruns the model discards the oldest buffered readings rather than stalling the client or growing without bound. That is the right trade for live telemetry and the wrong trade for anything you must not lose — which is the real decision behind the [QoS level](https://docs.oasis-open.org/mqtt/mqtt/v5.0/mqtt-v5.0.html#_Toc3901234) you pick. Must-process data wants QoS 1 with manual acknowledgement; lossy-but-fresh data wants QoS 0 with a bounded buffer.

## Feeding the model

The client hands you `args.PublishMessage.Payload` as a `byte[]`, which is what most sensor encodings want anyway — Protobuf, CBOR, or a raw binary frame. Decode it into whatever your model expects and call your inference runtime; on .NET that is usually [ML.NET](https://learn.microsoft.com/dotnet/machine-learning/) or the [ONNX Runtime](https://onnxruntime.ai/). Keep that call synchronous-per-reading in the QoS 1 path so the acknowledgement genuinely reflects that the model consumed the data. If a reading is text — JSON, say — `args.PublishMessage.PayloadAsString` saves you the decode.

## End to end

A production edge consumer combines the pieces: a stable client ID, manual acknowledgement, a receive maximum sized to how many inferences you want in flight, and inference that gates the acknowledgement.

```c#
using HiveMQtt.Client;
using HiveMQtt.Client.Options;
using HiveMQtt.MQTT5.Types;

var options = new HiveMQClientOptionsBuilder()
    .WithBroker("broker.hivemq.com")
    .WithClientId("edge-inference-01")
    .WithManualAck()
    .WithReceiveMaximum(8)
    .Build();

var client = new HiveMQClient(options);

client.OnMessageReceived += async (sender, args) =>
{
    try
    {
        var reading = SensorReading.Decode(args.PublishMessage.Payload ?? Array.Empty<byte>());
        await model.InferAsync(reading).ConfigureAwait(false);
    }
    finally
    {
        // Acknowledge even on failure so one bad frame doesn't wedge the stream;
        // route the failure to your own dead-letter handling instead.
        await client.AckAsync(args).ConfigureAwait(false);
    }
};

await client.ConnectAsync().ConfigureAwait(false);
await client.SubscribeAsync("sensors/+/telemetry", QualityOfService.AtLeastOnceDelivery)
            .ConfigureAwait(false);
```

That consumer pulls readings only as fast as the model clears them, keeps a bounded number in flight, and never spawns unbounded work — the broker does the throttling because you told it how much you can take.

## Wrapping up

The trick to streaming sensor data into an edge model is to stop thinking about the transport and start thinking about the rate. Let MQTT 5 flow control and manual acknowledgement make the model's throughput the intake rate for anything you must process, use a bounded buffer with an explicit drop policy for anything you can afford to lose, and pick the QoS level deliberately for each stream rather than defaulting it. Get that right and the same code holds up whether the model runs in microseconds or seconds.

If you're wiring the HiveMQ C# client into an edge pipeline and hit a pattern this doesn't cover, open an issue on the repo — the more real workloads we see, the better the client gets at carrying them.

## Further reading and implementation resources

**The client**

- [HiveMQ MQTT Client for .NET — source](https://github.com/hivemq/hivemq-mqtt-client-dotnet) and [documentation](https://hivemq.github.io/hivemq-mqtt-client-dotnet/)
- [Install from NuGet: HiveMQtt](https://www.nuget.org/packages/HiveMQtt)
- [Subscribing](https://hivemq.github.io/hivemq-mqtt-client-dotnet/docs/hivemqtt/subscribing) · [Events](https://hivemq.github.io/hivemq-mqtt-client-dotnet/docs/hivemqtt/events/Examples) · [Manual acknowledgement](https://hivemq.github.io/hivemq-mqtt-client-dotnet/docs/hivemqtt/how-to/manual-ack) · [Options builder](https://hivemq.github.io/hivemq-mqtt-client-dotnet/docs/hivemqtt/reference/client_options_builder)
- [Message ordering (FIFO QoS 1/2 handlers, v0.44.0)](https://hivemq.github.io/hivemq-mqtt-client-dotnet/docs/hivemqtt/how-to/message-ordering)

**The protocol**

- [MQTT 5.0 — Quality of Service levels and protocol flows (§4.3)](https://docs.oasis-open.org/mqtt/mqtt/v5.0/mqtt-v5.0.html#_Toc3901234)
- [MQTT 5.0 — Flow Control (§4.9)](https://docs.oasis-open.org/mqtt/mqtt/v5.0/mqtt-v5.0.html#_Toc3901251)
- [MQTT 5.0 — Receive Maximum (§3.1.2.11.3)](https://docs.oasis-open.org/mqtt/mqtt/v5.0/mqtt-v5.0.html#_Toc3901049)

**Edge inference in .NET**

- [ML.NET](https://learn.microsoft.com/dotnet/machine-learning/) · [ONNX Runtime](https://onnxruntime.ai/) · [System.Threading.Channels](https://learn.microsoft.com/dotnet/core/extensions/channels)

**A broker to test against**

- [HiveMQ Community Edition](https://github.com/hivemq/hivemq-community-edition) — run one locally in Docker
