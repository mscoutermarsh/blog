---
layout: post
title: My Top 3 Rails Best Practices
---
Here are my top 3 suggestions to improve your Rails codebase.

1. Learn the **presenter** and **service object** patterns. This allows you to break down complex logic into small, self contained and easily tested classes. This makes writing/changing/reading code easy. Remember to namespace things and create clear separation between different objects. Learn more here: [7 Patterns to Refactor Fat ActiveRecord Models](http://blog.codeclimate.com/blog/2012/10/17/7-ways-to-decompose-fat-activerecord-models/)

2. Roughly follow Sandi Metz rules: [Sandi Metz' Rules For Developers](https://robots.thoughtbot.com/sandi-metz-rules-for-developers). No need to follow them exactly. But use them as a guideline. Small models/short methods. Methods should do 1 thing only. You can use something like Rubocop to force yourself/the whole team to follow these rules.

3. Don’t be too clever. What I mean by this is: **Keep it simple and follow patterns**. Don’t add complexity prematurely. You may think you need that fancy event handler/rabbitmq/etc Ruby gem - but it can probably wait. I like to follow the rule of “do I have 3 problems I can solve with this?” before I add in something non-standard (I think I stole this idea from Sandi Metz’ book: [POODR](http://www.poodr.com/)).
