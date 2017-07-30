---
layout: post
title: Angry commit logs
date: 2012-12-15 18:22:00.000000000 -08:00
---
Here's a fun game.
How angry is your commit log?
```
git log --all | grep -o 'wtf|fuck|shit' | wc -l
```
