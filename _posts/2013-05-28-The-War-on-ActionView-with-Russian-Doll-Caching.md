---
layout: single
categories: ["Tracelytics", "Ruby"]
title: "The War on ActionView with Russian Doll Caching"
header:
  teaser: /assets/images/posts/tracelytics-layer-overview.png
---

**Update**: This article was originally featured on AppNeta's Application Performance Blog which unfortunately no longer exists.  In 2016, the TraceView product was sold to [SolarWinds](https://traceview.solarwinds.com/traceview/).


> Rails 4 is out featuring Russian Doll caching (AKA Cache Digests).  In this article, I apply Russian Doll caching to one of my poorer performing 
Rails 3 pages using the [cache_digests](https://github.com/rails/cache_digests) gem.

ActionView templates are great.  They are easy to code, manage and extend but the one thing they are not is _fast_...at least not out of the box.

_In this article, I'll be using AppNeta's [TraceView](http://www.appneta.com/products/traceview/) to time ActionView performance.  If you haven't used TraceView before, checkout my previous article [Instrumenting Ruby on Rails with TraceView](http://blog.gameface.in/post/48724980929/instrumenting-ruby-on-rails-with-traceview-in-under-10)._

### ActionView is Slow; Pitfalls Ahead

ActionView puts forth a great development pattern of layouts, views and partials that is easy to understand, implement and maintain but that comes at a cost: The rendering process is complex and slow.



![](/assets/images/posts/gameface+traceview+top+urls+layers+2013-05-27+at+21.01.47.png)

[Full-size image](/assets/images/posts/gameface+traceview+top+urls+layers+2013-05-27+at+21.01.47.png)

The screenshot above shows the timings for the `Users#show` URL on [Gameface](http://gameface.in).  The [page in question](http://gameface.in/u/nosis) is fairly straight forward containing four components: a topbar, sidebar, user details and a listing of game characters.

With no caching at all, the ActionView layer averages roughly ~365ms for this URL.  This represents over 80% of average request processing time and dwarfs all of the other layers combined.

![](/assets/images/posts/traceview_poor_performer_trace_detail.png)

[Full-size image](/assets/images/posts/traceview_poor_performer_trace_detail.png)

In terms of performance, ActionView is a weapon of mass destruction and is the low-hanging fruit for improvement.

### Russian Doll Caching

Russian Doll caching is a type of nested fragment caching that auto expires fragments when object timestamps change.

You still make the same calls as previous fragment caching schemes in Rails:
```ruby
- cache @user do
  (user view data)

  - cache [ 'details', @user ] do
    (user details view data)

  - cache [ 'characters', @user ] do
    - @user.characters.each do |character|

      - cache character do
        (character view data)
```

With Russian Doll caching (AKA cache digests) unique cache keys are formed using an md5 stamp based on the timestamp of the object being cached:

```sh
views/users/3-20130530135425/7a1bb8bb15b02ee7aa69cec1d5f6f630
views/details/users/3-20130530135425/6f28ec6d31e7e3b73a575777d59e63ca
```

The advantage of this is that when objects are updated and outer fragments are automatically invalidated, nested fragments can be re-used.  (russian dolls)

A key requirement to this is that children objects should update the timestamps of their parent object by using ActiveRecord `touch` option.  This will allow for automatic cache invalidation and avoid serving stale content.

```ruby
class Character < ActiveRecord::Base
    belongs_to :user, touch: true
end
````

For a more thorough explanation of Russian Doll Caching, see Ryan Bates [cache digests episode](http://blog.remarkablelabs.com/2012/12/russian-doll-caching-cache-digests-rails-4-countdown-to-2013) from Remarkable Labs.

### Cache Friendly Page Layouts

When caching, it’s best to avoid caching logged-in content since the same caches will get served to all users regardless of any logged in status.

For effective (and less problematic) fragment caching, designing the page well to separate out logged in content is critical.

Below is the layout for the Gameface profile view before re-design. Logged in specific content and links are sprinkled throughout the page making it hard to divide it up into cache-able fragments.

![](/assets/images/posts/traceview-gameface-before_profile.png)

[Full-size image](/assets/images/posts/traceview-gameface-before_profile.png)

Properly caching this page as-is would be complicated and inefficient since we would have to tip-toe around logged-in content.

### The Redesign

To fix this, I re-organized the page to group the logged-in specific content into one specific area. With logged-in content out of the way, we are then free to cache the rest of the page in well-defined fragments.

![](/assets/images/posts/traceview-gameface-after_profile.png)

[Full-size image](/assets/images/posts/traceview-gameface-after_profile.png)

### The Results

With the page re-design and Russian Doll fragment caching applied to the large majority of the page, we now average a much better ~120ms for ActionView on that URL. A reduction of 265ms or 67% in average processing time.

![](/assets/images/posts/traceview-gameface_after_profile_avg.png)

[Full-size image](/assets/images/posts/traceview-gameface_after_profile_avg.png)

On top of the performance improvement, we also get automatic cache invalidation as object timestamps are updated. This greatly simplifies the whole system by not requiring cache sweepers or other tricks to invalidate stale caches.

### Summary

Russian Doll caching is a small but significant improvement over prior caching in Rails. When used effectively, it can greatly reduce server side ActionView processing and automatically expire stale fragments.

We took a previously un-cached URL with a poor page layout that was averaging ~365ms of processing in ActionView and reduced that number to ~120ms for a 67% performance improvement.

### Additional Considerations

When using fragment caching, note which backing cache store you are using. The default cache store in Rails is the filesystem but you can get even greater performance by backing your cache store with memcache or redis instead.

See the [redis-rails](https://github.com/redis-store/redis-rails) gem as an example.

### Cheatsheet

Separate out logged-in content from agnostic content for best cache coverage. Page design affects caching efficiency.

When possible, always call cache with the object being cached. Cache keys and their eventual invalidation will be keyed off of the timestamp on the object being cached.

```ruby
- cache @user do
  (user view data)

  - cache [ 'details', @user ] do
    (user details view data)
```

To cache an index of objects, you have to revert to manually passing in the `expires_in` option since there is no single object timestamp to key off of.

```ruby
- cache 'user list', expires_in: 30.minutes do
  (user list view data)
```
Update `belongs_to` model relationships with `touch: true` so that parent fragment caches can be invalidated when children are updated.

Collect timing data before and after to quantify and validate changes.
