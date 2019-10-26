---
layout: post
title: Heroku + Ruby + GitHub Package Registry
date: 2019-10-26 11:09 -0700
---
If you're using Gems published to GitHub Package Registry.

```ruby
source "https://rubygems.pkg.github.com/mscoutermarsh" do
  gem "omniauth-auth0", "2.1.0"
end
```

You need credentials to install them. An easy trick for doing this is using bundler's built in 
environment variable support.

Add the following config var to your Heroku app. And you'll be good to go.

```
heroku config:set BUNDLE_RUBYGEMS__PKG__GITHUB__COM=YOUR_GITHUB_USERNAME:YOUR_GITHUB_TOKEN
```

Grab your personal access token from: [github.com/settings/tokens/new](https://github.com/settings/tokens/new) (you'll want READ PACKAGE permission).

### That's it

Because you're probably googling for this. Here are some phrases we want indexed.

```
Authentication is required for rubygems.pkg.github.com.
```

and

```
bundle config rubygems.pkg.github.com
```
