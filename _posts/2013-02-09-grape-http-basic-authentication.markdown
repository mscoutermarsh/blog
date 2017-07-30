---
layout: post
title: Grape http basic authentication
date: 2013-02-09 11:53:38.000000000 -08:00
---
Grape makes it really easy to secure your API with <a href="http://en.wikipedia.org/wiki/Basic_access_authentication">http basic authentication</a>.

Here's a quick example on how to authenticate via HTTP basic with devise.

```ruby
class Api < Grape::API
  # /private
  resource :private do

    http_basic do |email, password|
      user = User.find_by_email(email)
      user && user.valid_password?(password)
    end

    # this method requires authentication!
    # private/topsecret
    post :topsecret do
      {message: "you've found the secret",
       user: request.env['REMOTE_USER']}
    end

  end
end
```

**You can test this with HTTPie:**
```
$ http --verbose -a username:password POST http://yourapi.com/private/topsecret
```
