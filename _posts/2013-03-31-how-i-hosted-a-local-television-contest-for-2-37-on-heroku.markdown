---
layout: post
title: How I hosted a local television contest for $2.37 on heroku
date: 2013-03-31 23:00:00.000000000 -07:00
---
<img alt="" src="https://dl.dropbox.com/u/18216283/blog/heroku_bill.jpg" width="674" height="149" /> Big spender.

There's my heroku bill after hosting the voting for a local television contest.<strong> $2.37</strong>. Over 40,000 people used the app over a period of 2 weeks.

I spent the same amount on coffee this morning.
<h1>What?! How?!</h1>
I've always been really interested in scaling and getting the best possible performance out of limited resources.

I had the opportunity recently to build out the voting back-end for a local television contest. Projects like this are fun because I had the flexibility to try out something new and a lot of people would be using it.
<h1>The Stack</h1>
I wanted to build an API to handle the voting and get the best performance out of it. Usually I'd use <a href="http://www.sinatrarb.com/">Sinatra</a> for this, but this time I chose to try out <a href="http://postrank-labs.github.com/goliath/">Goliath</a>, which is a non-blocking Ruby web server for building APIs. I also used <a href="https://github.com/intridea/grape">Grape</a> since it would make writing the API even easier.

I hosted it on <a href="http://heroku.com">Heroku</a>. My initial goal was to see if I could keep it to only 1 dyno (more fun with a challenge).

If you're not familiar with Heroku, a dyno is the equivalent of a small virtual server with 512mb of memory. They also give you your first dyno for free. So all I ended up paying for was the postgres basic database add-on.
<h1>Stress Test</h1>
Before releasing it, I did some stress testing with <a href="http://www.joedog.org/siege-home/">Siege</a>. I couldn't have this app failing when it went live.

<strong>212.45 transactions per second. </strong>

On only 1 heroku dyno.

WOW GOLIATH IS FAST.

<img alt="" src="https://dl.dropbox.com/u/18216283/blog/wow.gif" width="500" height="282" />
<div></div>
Each transaction was a single GET request, that hit the database (postgres) and returned the count of contest votes. Postgres was probably the slowest part of each request. I could optimize even further by caching with memcached. Maybe next time.

The 200+ transactions per second was <strong>way more</strong> speed than I needed for this app. And much more than I expected to get out of a single dyno.

I knew that the traffic for this app would be pretty sporadic. Highly dependent on when it was mentioned on TV. Without Goliath, I would have needed to use more dynos to account for the spikes in traffic. But with the performance I was seeing out of Goliath, I knew it could handle the peaks with just 1 dyno.

<a href="https://gist.github.com/mscoutermarsh/4982940">See the details of the Seige test here.</a>

If you'd like to try it out yourself, the app I used to run this contest was a modified version of <a href="https://github.com/mscoutermarsh/Mathy-Poll">Mathy Poll</a>. Which is open source and you can grab it on <a href="https://github.com/mscoutermarsh/Mathy-Poll">github</a>.
<div></div>
