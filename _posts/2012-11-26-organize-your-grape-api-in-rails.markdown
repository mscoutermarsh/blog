---
layout: post
title: Organize your Grape API (in Rails)
date: 2012-11-26 16:22:00.000000000 -08:00
---
Grape is a Ruby framework for developing API’s. It’s fairly easy to mount it onto an existing Rails application.

One of the challenges I had with it was organizing the file/class structure. The Grape documentation is a little weak in this area. If your API has more than just a few methods, it can get messy really quickly.

My Grape API is part of an existing Rails app. So all of my API classes are in the lib directory.

<strong>My file structure looks like this:</strong>

<img alt="" src="https://dl.dropbox.com/u/18216283/blog/Selection_042.png" />

Each aspect of the API is split into a different class. All within the same <strong>API</strong> <strong>module.</strong>

I mount my API in rails using root.

```ruby
# in rails routes.rb
mount API::Root => "/"
```

## root.rb

This is where I initialize the API and load in the other classes.

```ruby
#root.rb
module API
  class Root < Grape::API
    prefix "api"
	version 'v1', :using => :header, :vendor => 'vendor'
	format :json
	error_format :json

    # load the rest of the API
    mount API::Auth
    mount API::Tasks
    mount API::Lists
  end
end
```
## auth.rb
I use auth.rb to keep all of the authorization and security logic for my API (access tokens…etc…).


```ruby
# auth.rb
module API
  class Auth < Grape::API
    # /api/auth
    resource :auth do

      #
      # auth code goes here!
      #

      desc 'Returns pong if logged in correctly.'
      params do
        requires :token, type: String, desc: 'Access token.'
      end
      get :ping do
        authenticate!
        { message: 'pong' }
      end
    end
  end
end
```
## lists.rb
This is an example of all the API calls available for the Lists model within my Rails app.

```ruby
# lists.rb
module API
  class Lists < Grape::API
    # /api/lists

    # all of your methods go here!

  end
end
```
