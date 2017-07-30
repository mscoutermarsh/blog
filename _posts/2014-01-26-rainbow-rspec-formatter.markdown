---
layout: post
title: Rainbow RSpec Formatter
date: 2014-01-26 04:58:02.000000000 -08:00
---
I was really jealous of [Minitest's](https://github.com/seattlerb/minitest) rainbow test output.

So I made a version for RSpec!

[![rspec-rainbow](https://raw2.github.com/mscoutermarsh/rspec-rainbow/master/rspec-rainbow.jpg)](https://github.com/mscoutermarsh/rspec-rainbow/)

## How to use it...

Add this to your application's Gemfile:

```ruby
group :test do
  gem 'rspec-rainbow'
end
```

And then execute:

```ruby
$ bundle install
```

Or install it yourself as:

```ruby
$ gem install rspec-rainbow
```

Then, when running rspec:

```ruby
$ rspec --format Rainbow
```

If you want to use Rainbow by default, add it to your `.rspec` file.


```ruby
--format Rainbow
```

Enjoy! And [source is on Github](https://github.com/mscoutermarsh/rspec-rainbow).
