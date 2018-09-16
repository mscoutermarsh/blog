---
layout: post
title: Configuring Unicorn for Octopus
date: 2014-08-03 08:23:26.000000000 -07:00
---
[Octopus](https://github.com/tchandy/octopus) is a gem that makes it easy to setup database replication (primary/follower) & sharding in Rails.

When using Octopus with Unicorn, we need to make a couple configuration changes to ensure our additional DB connections are established before users start hitting our app.

First, we need to initialize the DB connections in our **after_fork** block.

```ruby
after_fork do |server, worker|
  Octopus.config['production']['master'] = ActiveRecord::Base.connection.config
  ActiveRecord::Base.connection.initialize_shards(Octopus.config)
end
```

Then in our **before_fork**, we need to drop all connections.

```ruby
before_fork do |server, worker|
  if defined?(ActiveRecord::Base)
    shards = ActiveRecord::Base.connection_proxy.instance_variable_get(:@shards)

    shards.each do |shard, connection_pool|
      connection_pool.disconnect!
    end

    ActiveRecord::Base.connection.disconnect!
  end
end
```

[Look here for a full example](https://gist.github.com/mscoutermarsh/5ea3e45cf36d2c446616) of this being used.

That's it! With those two minor changes, Unicorn will properly establish and close our additional DB connections.

##For more...
* Check out the [Octopus gem on GitHub](https://github.com/tchandy/octopus).
* Also, take a look at [Unicorn](http://unicorn.bogomips.org/).
