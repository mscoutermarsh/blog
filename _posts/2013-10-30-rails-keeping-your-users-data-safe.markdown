---
layout: post
title: Rails + Keeping your users data safe
date: 2013-10-30 13:04:02.000000000 -07:00
---
Everyone encrypts their users passwords before storing them in their database. But what about the rest of their data?

There's a really quick way to do this in Rails.

The <a href="https://github.com/attr-encrypted/attr_encrypted">attr_encrypted</a> gem transparently encrypts and decrypts your users sensitive data so that it's no longer stored as plaintext in your database.

It's easy enough to setup. In your gemfile, add:
```ruby
gem 'attr_encrypted'
```
Run bundle install. Now you can use the attr_encrypted method to encrypt any fields in your models.
```ruby
attr_encrypted :secret_field, key: 'your_secret_key'
```

In your database, the column name should be prefixed with **encrypted\_**. So in this example, I have an **encrypted_secret\_key** column in my database.

Attr_encrypted uses the key that you define to encrypt and decrypt the data. Since you should never have keys stored in your source code, this should really be pulled out into an environment variable.

I created a Concern to handle the encryption key for me, this way it can be shared across any model that I want to use encryption in. I also want to raise an error if I deploy my code to a production server without the environment variable set.

Create a concern:
```ruby
# app/concerns/encryption.rb
require 'active_support/concern'

module Encryption
  extend ActiveSupport::Concern

  def encryption_key
    # if in production. require key to be set.
    if Rails.env.production?
      raise 'Must set token key!!' unless ENV['TOKEN_KEY']
      ENV['TOKEN_KEY']
    else
      ENV['TOKEN_KEY'] ? ENV['TOKEN_KEY'] : 'test_key'
    end
  end

end
```

Now this can be used in any model by adding this line:

```ruby
include Encryption
```
For example, this is how I would use it in my ApiKey model.

```ruby
# app/models/api_key.rb
class ApiKey < ActiveRecord::Base
  include Encryption
  attr_encrypted :secret_token, :key => :encryption_key
end
```
