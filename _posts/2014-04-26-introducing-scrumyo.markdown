---
layout: post
title: Introducing ScrumYo!
date: 2014-04-26 18:57:48.000000000 -07:00
---
ScrumYo is a command line tool that shows you all of your recent GitHub commits and pull requests. It helps you get quickly prepared for your daily scrum/stand up meeting.

![scrumyo screen shot](https://raw.githubusercontent.com/mscoutermarsh/scrum_yo/master/scrumyo_example.png)

## What'd you get done yesterday?
Almost every morning I'd be scrambling through my Git logs to remember exactly what I finished yesterday. So I wrote ScrumYo.

> Hey, it's time for scrum, yo.
> *- My Co-workers*

## How to use it...
Install it like any other gem.

```ruby
$ gem install scrum_yo
```
Or, if you're using RVM, add it to your global gemset.

```ruby
$ rvm @global do gem install scrum_yo
```

Once installed, use the scrumyo command.
```ruby
$ scrumyo
```

The first use you'll be asked to login to your GitHub account (it even works with two-factor auth!).

Enjoy! Let me know what you think.

The [source is on GitHub](https://github.com/mscoutermarsh/scrum_yo).



