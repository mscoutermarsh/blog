---
layout: post
title: 'Heroku Tip: Pre-boot'
date: 2013-01-03 02:06:44.000000000 -08:00
---
When pushing new code to <a href="http://heroku.com">heroku</a>, it shuts down and reboots your dynos. The problem with this is that it can take a few seconds for them to come back online. This makes your application really slow for anyone currently using it.

The way to fix this is to use <strong>pre boot.</strong>
<blockquote>This Heroku Labs feature provides seamless deploys by booting web dynos with new code before killing existing web dynos.</blockquote>
A couple things to be aware of:
<ol>
	<li>You need more than 1 dyno running to use this.</li>
	<li>Beware if you need to run migrations. You'll be running 2 versions of your code for a brief period of time.</li>
</ol>
<span style="line-height: 13px"><a href="https://devcenter.heroku.com/articles/labs-preboot/">Read more here!

</a></span>
