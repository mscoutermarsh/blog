---
layout: post
title: Send email on your users schedule
date: 2012-11-16 15:08:00.000000000 -08:00
---
The best time to send your customer email is when they are awake… and at their computers. When your customers are distributed throughout the world it gets a little tricky.

We read somewhere that open rates at 9am and 4pm are significantly higher than any other time of the day. So I wrote this to send all of our users an email in the morning according to their timezone.

This schedules a background sidekiq job for 9:25am in the users timezone.

```ruby
# Set zone to the users time zone
Time.zone = user.time_zone

# convert time to UTC
user_time_to_utc = Time.zone.parse('2012-10-22 09:25:00').utc
puts "Scheduling for #{user.login} at #{user_time_to_utc}"

# Schedule the sidekiq job
SendPromoEmail.perform_at(user_time_to_utc,user.id)
```
