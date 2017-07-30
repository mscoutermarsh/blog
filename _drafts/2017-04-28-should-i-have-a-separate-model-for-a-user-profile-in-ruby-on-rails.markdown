---
layout: post
title: Should I have a separate model for a user profile in Ruby on Rails?
---
Every case is different. But I’d be wary of ever creating a dedicated model for a **UserProfile**. It sounds like a very general “throw everything here” type of model. Which is something you should avoid. It’s better to be really specific about the type of data being stored.

This is how I structure it:

For user profiles, generally the information shown on the profile isn’t exclusive to the users profile.

For example: Medium.com’s user profile shows all of the users **posts**, their **followers** and what they have **highlighted**.


I’d imagine their data model works like this. They have a User model, a Post model, a FollowerAssociationModel and a Highlights model.

(ya I know Medium doesn’t use Rails, but if they did).

For their profile page, they bring in data from several different sources.

Use the View Object Pattern

The best way I have found to do this in Rails is to use the View Object Pattern (aka Presenter Object). You can read more about it here: 7 Patterns to Refactor Fat ActiveRecord Models (search view object).

So what you’d do is create a class that encapsulates all the data you want shown on the profile page.

Something like this maybe:

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
(a bit of a contrived example, but hopefully you get the idea)

Then this makes it really easy to have one nice clean object to access in your controllers and views.

# some controller
def show
  @user = User.find(params[:id)
  @user_profile = UserProfile.new(@user)
 
  # render a view...
end
Now if you’re wondering things like, “but my user has a resume on their site and I want it on the profile page.”

In cases like that, it makes sense to dedicate a model to just that object. Then you’d have a Resume object.

It keeps things very explicit and when other people are working with your code they will know where you look.

Hope that helps!
