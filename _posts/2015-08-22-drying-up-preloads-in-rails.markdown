---
layout: post
title: DRYing Up Preloads in Rails
date: 2015-08-22 17:06:00.000000000 -07:00
---
As a Rails application grows, I’ve found that this pattern often emerges where we are repeating the same preloads across the application.

```ruby
Post.preload(:user, :category, comments: { user: :avatar }).where(something: true).limit(100)
```

Whenever we add an association, or make a change. We have to update our preloads in multiple places. If we forget one, we’ve just introduced a potentially severe performance problem in our application.

(If you’re not [familiar with includes/preloads/n+1's, read this](http://www.sitepoint.com/silver-bullet-n1-problem/).)

We can DRY up this code by extracting it to a scope on the model.

We’ve been using this pattern in Product Hunt’s codebase for a few months now and I like it a lot.

```ruby

scope :with_preloads, -> { preload preload_attributes }

class << self
  def preload_attributes
    [:user, :category, comments: { user: :avatar }]
  end
end
```


Now in our code, we can use our new scope like this.

```ruby
# Old 😞
Post.preload(:user, :category, comments: { user: :avatar }).where(something: true).limit(100)

# New & Improved 😎
Post.with_preloads.where(something: true).limit(100)
```

Another benefit of adding this scope to all of our models is that it starts to look strange to see a query without **with_preloads**. It acts as a nice reminder to double check everything is being correctly loaded.
