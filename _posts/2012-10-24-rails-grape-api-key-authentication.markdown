---
layout: post
title: Rails + Grape + API Key Authentication
date: 2012-10-24 12:45:00.000000000 -07:00
---
When adding a <a href="https://github.com/intridea/grape">Grape</a> API to an existing Rails application you have a few options for user authentication. Probably the simplest and most basic way to authenticate an API is by issuing API keys to your users.

## How it works...
<ol>
	<li>User submits their Username/Password to the API.</li>
	<li>API authenticates the user and returns an API Key.</li>
	<li>For all subsequent API requests, the user submits the key with their request. This key authenticates them.</li>
</ol>
This is great because it protects the users credentials by taking them out of every API call. The API Key can also expire. So if it is compromised, access will only be available for as long as the key is active.

## How to do it...

To start: I’m going to assume that you have an existing Rails application with a Grape API and user authentication.

**1. Create an API Key model**

Create a new model with the following attributes:

* access\_token
* expires\_at
* user\_id
* active

```ruby
rails g model api_key access_token:string expires_at:datetime user_id:integer active:boolean
```
**2. Add indexes**

Now take a look at the migration that rails just created for you. Add indexes on **api\_key** and **user\_id** since those will be queried often. It should look like this:

```ruby
class CreateApiKeys < ActiveRecord::Migration
  def change
    create_table :api_keys do |t|
      t.string :access_token,      null: false
      t.integer :user_id,          null: false
      t.boolean :active,           null: false, default: true
      t.datetime :expires_at

      t.timestamps
    end

    add_index :api_keys, ["user_id"], name: "index_api_keys_on_user_id", unique: false
    add_index :api_keys, ["access_token"], name: "index_api_keys_on_access_token", unique: true
  end
end
```


Make sure you run the migration
```ruby
rake db:migrate
```

**3. Generate token**

Go to your api key model (api_key.rb). Replace what you have there with the following.
```ruby
class ApiKey < ActiveRecord::Base
  attr_accessible :access_token, :expires_at, :user_id, :active, :application
  before_create :generate_access_token
  before_create :set_expiration
  belongs_to :user

  def expired?
    DateTime.now >= self.expires_at
  end

  private
  def generate_access_token
    begin
      self.access_token = SecureRandom.hex
    end while self.class.exists?(access_token: access_token)
  end

  def set_expiration
    self.expires_at = DateTime.now+30
  end
end
```

This takes care of the creation of the access_token and setting the expiration date.

Please note that I have this setup to belong to User. You might need to change that based on how your existing authentication is setup.

**4. Add Authentication helpers to Grape**

Now, you need to setup a way for the user to create a new API key and then use it for authentication for other API calls.

Add the following helper methods to your Grape API file.
```ruby
helpers do
    def authenticate!
      error!('Unauthorized. Invalid or expired token.', 401) unless current_user
    end

    def current_user
      # find token. Check if valid.
      token = ApiKey.where(access_token: params[:token]).first
      if token && !token.expired?
        @current_user = User.find(token.user_id)
      else
        false
      end
    end
end
```

**5. Issue and test API Keys**

Finally, you need a method to distribute and test api keys.

Here, I’ve implemented **POST /api/auth**. It takes in the users credentials and if correct returns a new key.

I’ve also implemented **GET /api/ping**. Which tests if the key is correct and returns “pong.”

```ruby
# /api/auth
resource :auth do

  desc "Creates and returns access_token if valid login"
  params do
    requires :login, type: String, desc: "Username or email address"
    requires :password, type: String, desc: "Password"
  end
  post :login do

    if params[:login].include?("@")
      user = User.find_by_email(params[:login].downcase)
    else
      user = User.find_by_login(params[:login].downcase)
    end

    if user && user.authenticate(params[:password])
      key = ApiKey.create(user_id: user.id)
      {token: key.access_token}
    else
      error!('Unauthorized.', 401)
    end
  end

  desc "Returns pong if logged in correctly"
  params do
    requires :token, type: String, desc: "Access token."
  end
  get :ping do
    authenticate!
    { message: "pong" }
  end
end
```
