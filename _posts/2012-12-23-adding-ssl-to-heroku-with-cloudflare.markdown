---
layout: post
title: Adding SSL to Heroku with Cloudflare
date: 2012-12-23 19:55:00.000000000 -08:00
---
**TLDR;** Adding SSL to your Heroku app is easier and less expensive if you use [CloudFlare](https://cloudflare.com).

## Heroku & SSL

All Heroku apps already support SSL by piggybacking on Heroku's SSL certificate (https://your-app-name.herokuapp.com). Unfortunately, if you need SSL on a custom domain, you'll have to buy Heroku's SSL endpoint add-on ($20/month) and your own certificate.

## CloudFlare

Instead of using Heroku's SSL endpoint, we can alternatively use [CloudFlare](https://cloudflare.com). CloudFlare is a CDN (content delivery network). They cache and serve your static assets, making your site faster and reducing load on your servers. With the Pro account ($20), they will also handle SSL termination for you.

![Cloudflare & Heroku](/assets/archive/images/2014/Sep/cloudflare_ssl-2x.jpg)

Cloudflare Pro is $20 a month. The same cost as the Heroku add-on, but it's easier to setup and you also get all of their CDN/security features (you should be using a CDN anyway).

## How to do it?

1. Head over to cloudflare.com & sign up. They guide you step by step.

2. In your DNS settings, you'll want to create a CNAME: **yourdomain.com** -> **yourapp.herokuapp.com**.

3. Finally, in your CloudFlare settings. Enable "Full SSL" for the domain.

![Full SSL](/assets/archive/images/2014/Sep/fullSSL.jpg)

### Using Rails?
The next thing to do is force all http requests in your application to go redirect to https.

In Rails, this can be done by adding the following line to config/environments/production.rb.

```ruby
config.force_ssl = true
```

Alternatively, for other Rack apps, you can use the <a href="https://github.com/tobmatth/rack-ssl-enforcer">rack-ssl-enforcer</a> gem.
