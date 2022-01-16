---
layout: post
title: Why infrastructure engineers prefer MySQL
date: 2022-01-16 14:34 -0500
---

For years I’ve been noticing this pattern of infrastructure engineers I really respect preferring MySQL and product engineers preferring Postgres. I’ve learned enough about this now that I think I can high level summarize my understanding of it.

Infrastructure engineers generally care most about reliability, failure scenarios, upgrades and never, ever, losing data. Product engineers care about that too, but they focus more on tools that make building user functionality easier.

When an infrastructure engineer tells you they prefer MySQL. It’s probably because for them it’s simpler/easier to operate it. Backups, replication, failover, upgrades. Accomplishing what is most important to them.

When product engineers prefer postgres, it’s usually because of something like postgis, jsonb/hstore. Some feature they can use in the app to build something faster.

I hope this helps in explaining why you often see the infra orgs of many large/high scale companies choose mysql. Different types of engineers value different things.
