---
layout: post
title: Optimizing Puma's Backlog for Heroku
date: 2014-07-12 14:53:26.000000000 -07:00
---
**Update, please read:** I no longer recommend lowering your Puma (or Unicorn) backlog on Heroku. This approach can cause more problems than it will solve. For more details, please read: https://gist.github.com/schneems/c07d93d5a4ade679bbc3

Mike (*1/15/2015*)

---

[Puma's](http://puma.io/) default backlog is set to 1024. That means that 1024 requests will have to pile up before Puma will start to refuse requests.

This causes problems on Heroku due do how the router [sends requests randomly](https://devcenter.heroku.com/articles/http-routing#request-distribution). Keeping the backlog low ensures that Puma will reject requests sooner for busy dynos and Heroku can reroute the request to another dyno that is more capable of serving it.

Unicorn has a [config option that allows us to set the backlog easily](http://unicorn.bogomips.org/Unicorn/Configurator.html#method-i-listen).

Puma recently gained the functionality to set backlog, but it is undocumented.

## How to do it...
If you look at this [Puma commit](https://github.com/puma/puma/commit/43b2b7342d09c5184614aa62be6d04a7e0eac0d3). You'll see that the backlog can be set by adding a query parameter to the binding option.

You can do this by adding these lines to your Puma config.

```ruby
# config/puma.rb
port = Integer(ENV['PORT'] || 3000)
backlog = Integer(ENV['PUMA_BACKLOG'] || 20)

bind "tcp://0.0.0.0:#{port}?backlog=#{backlog}"
```

([View the full example here.](https://github.com/mscoutermarsh/puma_heroku_example/blob/master/config/puma.rb))

Here is an example app that works on Heroku with the backlog config: [Example on Github](https://github.com/mscoutermarsh/puma_heroku_example).

**Note:** Make sure you're using a recent version of Puma for this to work. In this example I was using 2.8.2.

## For more...
A great explanation on why random routing & a deep backlog are bad:

* [Managing Request Queuing With Rails on Heroku]( http://devblog.thinkthroughmath.com/blog/2013/02/27/managing-request-queuing-with-rails-on-heroku/).
