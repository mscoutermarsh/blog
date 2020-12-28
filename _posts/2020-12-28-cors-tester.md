---
layout: post
title: CORS Tester
date: 2020-12-28 11:31 -0800
---

I wrote a little [CORS tester tool](https://cors-test.codehappy.dev) to make testing urls for CORS easier. Mostly I was interested in experimenting with Cloudflare workers more.
The entire site is deployed as a Cloudflare worker and served by a single js file. It worked real nicely for a small site. For a larger project, it
would likely benefit from a bit of a framework for separating out the views (HTML). The 30-40ms response times from anywhere are real impressive.

## [CORS Tester](https://cors-test.codehappy.dev). 
A tiny tool for checking if your CORS headers are setup correctly.

<img src="https://github.com/mscoutermarsh/cors-test/blob/main/screenshot.png?raw=true" width="500px" />
