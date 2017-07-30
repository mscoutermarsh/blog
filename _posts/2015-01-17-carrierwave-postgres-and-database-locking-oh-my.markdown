---
layout: post
title: CarrierWave, Postgres and Database locking, oh my!
date: 2015-01-17 16:31:22.000000000 -08:00
---
Are you seeing strange performance issues in your Rails app? Random slow requests? Deadlocks? Did you recently add a CarrierWave uploader?

If so, read on!

CarrierWave makes it easy for us to transfer and process images from a remote URL.
We can do it in just two lines of code... like so:
```ruby
user.remote_avatar_url = 'http://example.com/cat.jpg'
user.save!
```

**Note:*** In this example we have a `User` model with a mounted CarrierWave uploader, `Avatar`.*

While this is super easy. The image processing happens in an after save hook, meaning that it locks the relevant row in the database until processing is complete.

In this example, the image is a part of the `User` model. During the time the image is processing, our application will be blocked from making changes to that user. This is bad.

### See the lock...

The first step to fixing this is to start out by observing the current locking behavior. We can later repeat this with our new code to be sure it eliminates the lock.

For Postgres, we can use the following query to see the locks on our DB.

```sql
SELECT t.relname,l.locktype,page,virtualtransaction,pid,mode,granted
FROM pg_locks l, pg_stat_all_tables t
WHERE l.relation=t.relid order by relation asc;
```

For my test, I ran my current upload code in a Rails console (`$ rails c`). Quickly switched to a psql console and ran the locking query before the processing could complete. As expected, the query showed me that a **Row Exclusive Lock** was being placed on the `user`.

### Fix the lock...

Now that we have been able to replicate the lock. Let's add a new method that will process our remote upload without placing a lock.

```ruby
  # In user.rb or wherever your carrierwave uploader is mounted.
  def process_avatar_upload!
    return unless remote_avatar_url.present?

    file.download!(remote_file_url)
    file.store!

    # Update column in DB (instead of carrierwave object)
    attributes['avatar'] = CGI.unescape(file.path).split('/').last
    self.remote_avatar_url = nil
    save!
  end
```

Now we can use this method to process our uploads without locking.

```ruby
user.remote_avatar_url = 'http://example.com/cat.jpg'
user.process_avatar_upload!
```

If we run this code in a Rails console, along with our query for checking PG locks, we'll see that a lock is no longer placed while the image is processing.

This method was adapted from [Keith's solution in the CarrierWave wiki](https://github.com/carrierwaveuploader/carrierwave/wiki/How-to:-prevent-carrierwave-from-locking-the-db-row-of-your-object-while-it%27s-processing-the-image-attached-to-that-object). Thanks Keith!
