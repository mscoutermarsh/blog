---
layout: post
title: Setting up Redis on Cloud66 for Sidekiq
date: 2013-04-25 11:36:01.000000000 -07:00
---
<a href="http://cloud66.com/">Cloud66</a> will automatically install and setup Redis for you after analyzing your app.

By default Cloud66 will provide a REDIS_ADDRESS environment variable with the IP address to your redis server.

To get it running with Sidekiq, add the variable to your Procfile.
<pre>worker: env RAILS_ENV=$RAILS_ENV REDIS_URL=redis://$REDIS_ADDRESS bundle exec sidekiq</pre>
Sidekiq will recognize your redis server and you'll be up and running!
