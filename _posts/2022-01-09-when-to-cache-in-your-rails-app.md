---
layout: post
title: When to cache in your Rails app
date: 2022-01-09 09:38 -0500
---

I’ve been working on Rails applications for over 10 years. In that time I’ve figured out some guidelines on when and when not to use caching.

Years ago, I’d cache almost everything. When joining GitHub, I remember being shocked by how little caching was actually used for such a high traffic app. It made me think, maybe I’ve been approaching this wrong?

## Hardware and Rails are now faster
When I first started with Rails, CPU’s and Ruby were much slower. The rendering of views would often be a large factor in the over all request time. Caching the rendering of a view was almost always a big performance win.

This has changed. As Rails has gotten faster and the servers we run our apps on are more powerful, I’ve seen the time spent rendering views to be less of a factor. In fact, I’ve seen some views get faster by removing caching. Each cache call is a network request, these can add up and result in you spending more time talking to your cache than would have been spent rendering the page.

## Caching Guidelines: 

**1. Don’t cache by default**

Rails makes caching so easy that by default many of us will add it to every page in our apps. I encourage you try using no cache at all. Why? Caching is another layer of complexity. If your app is fast enough without it, that will make maintenance and debugging easier. Fixing a bug where caching may be involved is never simple.

Ship your feature first without any caching. Look at the performance of the feature, then only add some of its necessary.

Even then, look at other factors. Are you using a $5 server? Using better hardware might get you better results than relying on a cache.

**2. External service calls can be good to cache**

Any calls to external API's that change infrequently can often be a great place to add caching. External network calls are generally slow (30-100ms). This is a lot of time you can save.

Here's an example. On the [PlanetScale](https://planetscale.com) app, we show graphs of your database usage. The data for these graphs comes from another service outside the Rails app. This data is time based and updates every 5 mins. Any external requests to the service beyond once per 5 minutes is wasteful. This makes it a great candidate for caching the data.

<img src="{% asset_path cached-graph-data.png %}" width="500px" alt="cached graph data"/>

I found this pattern used as well in GitHub's codebase. The most cached data in the app is probably the Git data. It comes from an external service + does not change (based on the git sha).

**3. Computationally intense views**

Even with the increase in server performance, there are some cases where you may be doing something so complex that it's still worth caching it.

This past year I worked on schema diffs in PlanetScale. This view grabs the schema from two database branches, compares them and then also generates a syntax highlighted diff to be displayed in the UI. It's an expensive operation that also never changes once computed once. This is a great place to add caching.

![complex schema diff view to be cached]({% asset_path schema-diff.png %})

**4. Database queries can probably be left uncached**

It can be tempting to fix a slow database query by caching it. In a Rails app, your queries should be taking 1-10ms (including network time) on average. Compare this to the time to access a cache (1-5ms) and you're not seeing much of a win.

If you have slow queries, I'd recommend looking first at why they are slow and trying to improve them before adding caching. If you can fix the query, then you've just avoided adding complexity to the code.

**5. Caching to protect from abuse**

I always recommend adding some sort of request limit to applications (such as the rack-attack gem). This can protect you from bad actors.

In some cases though, you may want the requests to still be served, but without hitting an external service that may not be able to handle the traffic.

A pattern I've seen and liked is wrapping a call to an external service in a cache with a short TTL. The request parameters can be used as the cache key. If this endpoint is prone to abuse (rapid requests), this limits the number of requests that get through to the external service in the set amount of time. It's a cheap way to implement idempotent requests.
