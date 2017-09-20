---
layout: post
title: 'Rails: Should I have a separate model for a user profile?'
---
Every case is different. But I'd be wary of ever creating a dedicated model for a **UserProfile**. It sounds like a very general “throw everything here” type of model. Which is something you should avoid. It's better to be very specific about the type of data being stored.

## Modeling Medium's user profile

For user profiles, generally the information shown on the profile isn't exclusive to the users profile.

**For example:** Medium.com's user profile shows all of the users **posts**, their **followers** and what they have **highlighted**.


I'd imagine their data model works like this. They have a `User` model, a `Post` model, a `FollowerAssociationModel` and a `Highlights` model.

(yes, I know Medium doesn't use Rails, but if they did).

For their profile page, they bring in data from several different sources.

## Solution: Use the View Object Pattern

The best way I have found to do this in Rails is to use the `View Object Pattern` (aka Presenter Object). You can read more about it here: **[7 Patterns to Refactor Fat ActiveRecord Models](http://blog.codeclimate.com/blog/2012/10/17/7-ways-to-decompose-fat-activerecord-models/)** (search view object).

Create a class that encapsulates all the data you want shown on the profile page.

```ruby
class UserProfile
  attr_reader :user

  def initialize(user)
    @user = user
  end

  def tagline
    # example of some html conversion
    @tagline ||= ConvertTagsToLinks.run(user.tagline)
  end

  def posts
    @posts ||= user.posts.order_by_featured_date
  end

  def followers
    @follows ||= user.followers.ordered_by_popularity
  end

  ## ect...
end
```
(a bit of a contrived example, but hopefully you get the idea)

Then this makes it really easy to have one nice clean object to access in your controllers and views.

```ruby
# some controller
def show
  @user = User.find(params[:id)
  @user_profile = UserProfile.new(@user)

  # render a view...
end
```

Now if you’re wondering things like, “but my user has a resume on their profile and I want it on the profile page.”

In cases like that, it makes sense to dedicate a model to just that object. Then you'd have a `Resume` object.

It keeps things very explicit and when other people are working with your code they will know where to look when making changes.
