---
layout: post
title: Using Hstore with Rails 4
date: 2013-09-22 12:59:40.000000000 -07:00
---
I have a big crush on Hstore and its new native support in Rails 4.

If you aren't familiar with HStore. It basically gives you a schema-less key/value datastore in your PostgreSQL DB.  This allows you to store the equivalent of a hash in a database column.

## How to do it...

First, you need to enable the hstore extension in the PostgreSQL database. You can do this with a migration.

```ruby
class AddHstore < ActiveRecord::Migration
  def up
    enable_extension :hstore
  end

  def down
    disable_extension :hstore
  end
end
```

Next, since hstore is now a natively recognized datatype in Rails, you can add an hstore column to any existing model. Here I'm adding a "settings" column to my user model.

```ruby
class AddSettingsToUser < ActiveRecord::Migration
  def up
    add_column :users, :settings, :hstore
  end

  def down
    remove_column :users, :settings
  end
end
```
Finally, you can define accessors for your hstore keys in your model. Validations work just like they would for any other column in your model.

```ruby
class User < ActiveRecord::Base
  # setup hstore
  store_accessor :settings, :favorite_color, :time_zone

  # can even run typical validations on hstore fields
  validates :favorite_color,
    inclusion: { in: %w{blue, gold, red} }

  validates_inclusion_of :time_zone,
    in: ActiveSupport::TimeZone.zones_map { |m| m.name },
    message: 'is not a valid Time Zone'

end
```

## Handling data types
One thing to look out for is storing booleans in hstore. Hstore will convert booleans to strings. A quick solution to this is overwriting your getter method to convert them back to a boolean on read. Here's an example:

```ruby
# convert string to boolean. hstore only stores strings
def name_of_some_hstore_key
  return (super == 'true') if %w{true false}.include? super
  super
end
```

## What's it useful for?
Hstore is really useful for saving attributes on models.

If you store settings for your users, you'd typically do this in a separate model (or on the User model). Each setting would be an additional column. Instead of adding columns each time you want to create a setting, you could instead use a single HStore column. It's much more flexible and doesn't require migrations each time we want to store something new.
