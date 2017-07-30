---
layout: post
title: Using Bullet Gem with Sinatra
date: 2014-01-04 03:21:01.000000000 -08:00
---
To get the [Bullet](https://github.com/flyerhzm/bullet) gem running correctly on a Sinatra app.

**Add bullet to your gemfile.**

```
gem 'bullet', require: false
```

Then in either your `config.ru` or `app.rb`. Add the following:

```ruby
require 'bullet'

Bullet.enable = true
Bullet.alert = true
Bullet.bullet_logger = true
Bullet.console = true

use Bullet::Rack
```

Now, if you start your app, you should get warnings from Bullet for any N+1's or unused eager loading.
