---
layout: post
title: Why not use the most "hype" stack?
date: 2022-01-14 09:52 -0500
---

Got a great question in Twitter DMs. 

> How do you stay motivated on working with boring/non-hype tech? I’m always hyped about a new stack and can’t make progress building a SAAS.

It’s hard out there. If you read twitter, hacker news, etc, there is always something new and better coming out. It’s distracting and many people are spinning their wheels rather than making progress towards their goals.

**Answer this question:** which would you rather have? A single index.php file that generates $10k a month? Or a React/GraphQL/Serverless app that has a lot of GitHub stars?

I’ll take the $10k.

## Valuable engineers create business value with technology

Read that ^ multiple times and internalize it. That’s the most straight forward path I’ve seen engineers become successful (measured by $$). Notice which is mentioned first here and prioritize. Business value > technology. When making a tech decision there are many different factors you need to weigh against. In the end, you need to do the ROI (return on investment) calculation in your head and pick which will result in the best outcome.

It might sound like I’m saying "always pick whatever ships product the fastest". This is often true. But not always. Some times you’ll choose to go a little slower to gain something else, such as reliability or scale. Just make sure those things actually matter before making that choice.

## Be clear with your goals

If your goal is to build a SAAS product. Then make your tech choices in a way that prioritizes getting your product built quickly. When I have goals I like to phrase them like this “6 months from now, if I don’t have X done, how will I feel?”. That clarifies what you need to do now.

**This means:** use the tech you already know and build with that. Even if hacker news will think you’re boring. I promise you that your potential future customers will not care how you built it, so long as it works. Once you have a stable business (this is incredibly hard!) then you’ll have gained yourself the freedom to maybe include a few newer tech choices that you’re interested in learning.

In my experience, it feels a lot better sitting back and making money from a product than it does impressing other engineers with the cool tech I used.

## Hype tech always exists, you won’t miss out

I’ve only been in this industry for ~11 years and can tell you, the hype tech of the day will always be there. It happens over and over again. Some new thing is invented, people rush to use it, then it either slowly becomes the normal or dies off. I used to try out everything new thing because I was worried about falling behind. After burning myself enough times, I stopped. And guess what, everything has been fine.

This mistake happens so commonly. I bet you that hundreds of millions in VC $ have been wasted by young companies coverting their UI’s to React when it first came out. Only to have their company go under a couple years later.

If a new piece of tech truly is useful, it will stick around for years. It’s much easier to adopt something that has been around for a while than using something new. Let the early adopters weed out all the bugs.

## When to use hype tech

I do think it’s smart to occasionally try new things out. Maybe once or twice a year you see something interesting. For example: when Cloudflare workers were released, I really wanted to learn them.
I scoped a super tiny weekend project out and built it. From that, I learned their value and drawbacks. With that knowledge, I found a perfect use case for them doing “real work” and added them in as a small piece on a real app (that used boring technology).

The result is having a stable core. With a sprinkle something fun and new that has shown value.

## My boring stack
Personally, this is what I default to because I've used it for years and know I can build almost anything with it.

- Ruby on Rails
- UI: tailwind or bootstrap
- JavaScript: as little as possible, or use web components
- Servers: [Heroku](https://heroku.com)
- Database: [MySQL](https://planetscale.com)
- Cache: memcached

## Summary

- Be clear on your goals
- Business value is more important than technology
- You won’t miss out, a new thing is always coming

## More reading
- [Boring Technology Club](http://boringtechnology.club/)
