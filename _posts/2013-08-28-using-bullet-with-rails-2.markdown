---
layout: post
title: Using Bullet with Rails 2
date: 2013-08-28 10:58:07.000000000 -07:00
---
The <a href="https://github.com/flyerhzm/bullet">Bullet</a> gem helps you optimize your Rails app by pointing out where you have N+1 queries in your code (check out this <a href="http://railscasts.com/episodes/372-bullet">RailsCast</a> if you have no idea what that means).

I recently needed to set it up for an older Rails app, still running Rails 2. Here's how I did it.

Add the gem to your gemfile. This version of bullet works with Rails 2.
```ruby
gem 'bullet', '1.7.6'
```

Next, you need to add the bullet initializer to your development.rb.
```ruby
config.after_initialize do
  Bullet.enable = true
  Bullet.alert = true
  Bullet.bullet_logger = true
  Bullet.console = true
end
```

And that's it! You'll now get alerts whenever N+1 queries are encountered.
