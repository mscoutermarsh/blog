---
layout: post
title: Mathy-Poll - Voting API with Math CAPTCHA
date: 2012-12-19 17:02:00.000000000 -08:00
---
<p><a href="https://github.com/mscoutermarsh/Mathy-Poll"><img src="https://dl.dropbox.com/u/18216283/blog/mathy-poll.jpg" /></a></p>
<p><a href="https://github.com/mscoutermarsh/Mathy-Poll">Mathy-Poll </a>is a voting API that asks basic math questions before recording the vote.</p>
<p>It was originally written in Sinatra when I was still fairly new to Ruby. Today I had some free time and decided to move it over to Grape/Goliath.</p>
<p><a href="https://github.com/intridea/grape">Grape</a> is a micro-framework that&#8217;s specifically for building API&#8217;s. Which makes it perfect for this project. It gives you a bunch of API specific things that Sinatra doesn&#8217;t. Such as&#8230;</p>
<ul><li>even simpler syntax</li>
<li>API versioning</li>
<li>parameter validation</li>
<li>better exceptions</li>
</ul><p>And then <a href="https://goliath.io">Goliath</a> is a really fast ruby framework that I hadn&#8217;t played with yet. So this was a good  excuse to try something new.</p>
<p>I&#8217;ll be including it in one of my projects soon. I&#8217;m interested in seeing how well it will stand up against a ton of traffic.</p>
<p>You can see<a href="http://mathy-poll.herokuapp.com/"> it in action here</a>.</p>
<p>Source is on <a href="https://github.com/mscoutermarsh/Mathy-Poll">Github.</a></p>
