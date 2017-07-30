---
layout: post
title: How do I know when to upgrade memcached?
date: 2015-03-19 10:11:50.000000000 -07:00
---
I grabbed a great screenshot the other day of what it looks like when a memcached instance isn't large enough and needs to be upgraded.

Hopefully this helps others in determining if/when they need more RAM for memcached.

This is from New Relic's memcachier plugin. Most memcached providers have some dashboard similar to this. The numbers you're interested in are the **hit ratio %** and **number of evictions**.

![]({% asset_path archive/images/2015/Mar/memcached.png %})

**hit ratio %**: Percentage of time your application requests data from memcached & finds it.

**evictions**: When memcached runs out of memory. It will delete the oldest/least accessed data to make room for new data.

If you see hit ratio go down and evictions going up. This means that memcached is evicting data that is still in use by your application. Meaning, it's time to add more RAM.

