---
layout: post
title: Sharing data across controllers - Angular JS & CoffeeScript
date: 2013-04-05 23:40:00.000000000 -07:00
---
Here's an easy way to share data across multiple controllers in Angular JS.

First, setup a service that contains shared variables that you want to be accessible across controllers. I create getters/setters for each piece of data. This is a simple example.

```coffeescript
# shared attributes service.
app.factory "Shared", ($rootScope) ->
  shared = {}
  shared.attribute_one = []
  shared.atrribute_two = []

  # a default value
  shared.some_array= ["Hello","World"]

  # getters/setters
  shared.get_attribute_one = ->
    return shared.attribute_one

  shared.set_attribute_one = (new_value) ->
    shared.attribute_one = new_value
    # send out a broadcast, letting the controllers know this value is updated
    $rootScope.$broadcast('attribute_one')

  shared.get_attribute_two= ->
    return shared.attribute_two

  shared.set_attribute_two= (new_value) ->
    shared.attribute_two = new_value
    $rootScope.$broadcast('attribute_two')

  return shared
```

Now, to make use of the Shared service, pass it into your controller. Some examples of how to use it are below.

```coffeescript
@MyController = ($scope, Shared) ->
  $scope.attribute_one = Shared.get_attribute_one()
  $scope.attribute_two = Shared.get_attribute_two()

  $scope.some_function = (new_value) ->
    # A function that can be called from the DOM/controller
    Shared.set_attribute_one(new_value)

  # When attribute_one is updated in another controller. Retrieve new value
  $scope.$on('attribute_one', ->
    $scope.attribute_one = Shared.get_attribute_one()
    alert "attribute_one was updated with: #{$scope.attribute_one}!!!"
  )
```
