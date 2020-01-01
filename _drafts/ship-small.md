---
layout: post
title: How we use "ship small" to rapidly build new features at GitHub
---

When building software, we often hear this advice. "Ship small! fast!" and "ship to learn!".

It's easy to say, but difficult to do.

This process is personally one of my favorite parts of building software and I've found it to be very effective for teams I'm on.

**First up, some important points:**

- you need to be able to deploy and rollback quickly (if you only deploy a couple times a week, this won't work well)
- you need feature flags to gate code and ship only to specific users
- this is a general example of how teams I've been on work (GitHub is a big company and individual teams do what works best for them)
- I'm an engineer, this is an engineering centric view of the process

## Let's build a new feature
Ok, because I need an example, let’s pretend we are building a new feature for a web app that has a UI/frontend + backend. We have 2 engineers, 1 eng manager, 1 designer & 1 product manager working on this project.

### Project planning, define the minimal scope
To start, the lead engineer, product manager and eng manager work together to define minimal scope. We determine what parts are a MUST for this to be valuable to users.
Once this minimal scope is defined, we'll share it to get feedback. There is probably even more that can be cut or deprioritized by doing a 2nd pass.

The lead engineer then does a break down of tasks and priorizes them on a project board. Their job here isn't to be the person that figures out every technical detail. Their job is to find enough info to break the project up into small enough pieces people can get started with.

During planning we aren't spending hours hammering out small details. Perfect upfront planning can be the enemy of shipping. We know that as long as we have the minimal scope defined, we can always iterate and improve from there. Software is never complete.

There will still be things to figure out, but we don't need to wait to get started.

### Start writing code
Getting started as soon as possible is important. I've never seen any software project go exactly to plan, as soon as code starts to be written, we know we will uncover new information that may change our approach.
We need to be flexible and roll with it as we learn more. As individuals find problems, they document them in new issues and it gets added to the project plan.

**The first pull request - the boilerplate**  
Since this feature has a UI. One engineer will do the boilerplate setup for it. This is a blank page, that has a URL and is behind a feature flag. It's essentially a "hello, world" for the feature.

This should be a very quick task. They will open the pull request, tag everyone else working on the feature, and we get it shipped to production. Everyone on the team gets added to the feature flag, they can now see the blank page in production.

I **cannot emphasize enough how important this first pull request is**. 

Once it's merged we've now made it much easier for others to build on top of it without running into each other. If your goal is to "ship small", I believe it's critical
to start the project off that way as well. The first change is the example set for the changes to come after it. It's now culturally accepted to be shipping things to production
that aren't complete yet.

**We can now have parallel workstreams**  
- 1 engineer can start working on the backend that feeds data to this page.
- Another engineer can start working on the frontend. 
- The designer (who codes) can start adding markup + css to the blank page.

We now have everyone able to contribute and we're able to ship partially completed work to production confidently (remember, it's behind a flag).

As small pieces of work are completed, everyone is opening pull requests, getting reviews from each other and merging to production. The shared context of the project is high.

If we need reviews from other teams (security for example), these small PR's are fairly low effort to review for outside teams. Making it likely we get feedback faster.

**This is important because:**  
- it allows people to work async (we are remote/different timezones).
- each pull request is small and reviewed by everyone else. Making them easy to review and keeping everyone aware of progress/context
- Project stakeholders can get feature flagged in and view production to see progress (and give feedback!)

**Never get blocked, start ASAP**  
Often when building software we'll find ourselves in a situation where we believe "we need X from other person" before we can start. I've never found this to be 100% true.
I have seen software projects be stuck in a holding pattern where engineers are waiting on each other (this can burn days, or even weeks of time). We can generally always start something.

For example, maybe you need data from the API before starting the frontend. Stick it in a JSON file and fake the API. You're then unblocked and can get started. You'll then learn things about the data you need and will be able to provide this information to the developer who is working on the API. This will make their job easier and probably get you the API endpoint faster and exactly how you need it.

### Production code is the best code
There's a reason we want to get changes into production as fast as possible.

When code is successfully merged and deployed to production, we've now eliminated a bunch of uncertainties about it. We know it passes CI, we know it won't bring the site down.

Even though it's behind a flag, there's always the possibility of something going wrong. Getting it into production increases our trust of it and makes it easier for the team
to build on it moving forward. Confidence is a huge driver of momentum. We need both if we're going to be able to build this feature quickly.

### Feedback loop
As this project progresses, we are seeing changes in production daily. Once the feature is somewhat working, we have a good opportunity to start inviting more people into the process. 
I like to share with people from security, documentation, support, sales, biz dev etc. Add them to the feature flag and ask them for feedback.  

Remember how we started with a very basic project plan? At this point we have feedback coming in, we turn that feedback into new issues (tasks on the project board) and it feeds right
back into the development process.

We are now ITERATING on the feature and making it better. Iterating is easy for the team because we've been making small changes all along. People have a shared context
of how everything works, so contributing is easy. We have confidence because the majority of the code is in production.

Personally this is my favorite phase of every project. If things were setup correctly from the start, there should be a lot of momentum driving everything forward at this point.

This process of feedback -> code -> ship continues to loop until we have something we can ship to real users.

### Shipping to real users
Once the team has accomplished the minimal scope, it's time to start getting this new feature out to real users.
The high level process we follow is "Staff ship" -> get feedback -> "General availability". There can be a lot of other details here (documentation, capacity, security, abuse) that depend on the feature.

The "Staff ship" is when we make the feature available to every employee and ask for feedback. Our company is large enough that this works really well for uncovering 
potential problems. During this time we also watch instrumentation more closely for potential errors/performance problems. The ways people use our product is highly
variable and we can't anticipate everything.

"General availability" is when we make the feature available to everyone. We have tools available to let us roll it out slowly to a % of users. As we do this we continue to
monitor things closely. If we see serious problems we can always turn it off, fix the problem and then continue the roll out.

### Principles

This was a pretty generalized view of how we work and the details will vary from project to project. But I think I can distill it down to a few important points.

- Start by defining the MVP. Review and cut scope until it truly feels minimal.
- Software is never done. We are OK with shipping things that aren't perfect. We can always ship another PR to improve it.
- Always be unblocking. Make it easy for people to be contributing at all times (even if alone in a separate timezone).
- The project plan starts minimal and evolves as we ship code. Invite others early to give feedback, incorporate that into the plan as the project progresses.
