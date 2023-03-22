---
layout: single
categories: ["Deep Learning", "Python", "AI"]
header:
  teaser: /assets/images/posts/sc2ai-example.gif
title: "Using StarCraft II for AI Research"
---

Building a neural network that does things other than linear regression, MDP or identifying cats in images is apparently non-trivial.  Not because of the complexities of the AI/DL/RL subject matter, but because working with raw datasets is atrociously boring.  After the books and blogs with their examples, whats next to apply a neural network to?  Games. 😎

For me, the following were candidates but each have been a challenge to get up and running either because of unsupported platforms, blocking bugs or just too complicated:

* Use the [BizHawk](https://github.com/TASVideos/BizHawk) game emulator to have [a neural net play Super Mario Brothers](https://www.youtube.com/watch?v=qv6UVOQ0F44) _(Runs on Windows only)_
* Nvidia's [Isaac SDK and Sim](https://developer.nvidia.com/isaac-sdk) _(AFAICT requires access to an Nvidia card)_
* FaceBook's [Habitat](https://aihabitat.org/) _(Seemed too advanced with vision processing for what I wanted to do)_
* [Vincross Mind SDK](https://developer.vincross.com/en) with SIM _(Seemed too vertical and specific to the Hexa platform)_
* [OpenAI Gym](https://gym.openai.com/) _(This is a nice platform that I plan to spend more time with)_

Then I found StarCraft II and AlphaStar.  In 2016, [DeepMind and Blizzard teamed up](https://deepmind.com/blog/deepmind-and-blizzard-release-starcraft-ii-ai-research-environment/) to make StarCraft 2 an AI research platform.  See this [Youtube video](https://www.youtube.com/watch?v=DpRPfidTjDA) showing DeepMind's AlphaStar play SC2 tournament pros.

Installation is a breeze:

1. [Install](https://www.blizzard.com/en-us/download) Battle.net & StarCraft II (the free version)
2. `pip install pysc2`
3. `python -m pysc2.bin.agent --map Simple64`

And you get a running agent and a special dashboard for monitoring:

![](/assets/images/posts/sc2ai-example.gif)

The ease and simplicity of getting this all working and running makes this a great option.  Now for the fun stuff...


Related Links:

* [DeepMind's Python package for StarCraft II](https://github.com/deepmind/pysc2) (pysc2)
* [sc2ai Wiki Page](http://wiki.sc2ai.net/Main_Page)
* [AI defeats humans at StarCraft II](https://bdtechtalks.com/2019/01/28/deepmind-alphastar-ai-starcraft-2/)
