---
layout: post
title: Auto scaling Heroku with HireFire
date: 2012-12-18 14:04:23.000000000 -08:00
---
<p>I run all of my applications on <a href="http://heroku.com">heroku</a>. If you&#8217;re not familiar with heroku&#8230; it&#8217;s a PaaS (platform as a service) that takes care of all your infrastructure for you. It lets me scale an applications resources up and down as needed.</p>
<p>For <a href="http://taskk.it">Taskk</a>, our traffic occasionally spikes and we need to scale up quickly. Heroku is great for this, but it requires that one of us is always watching the site. For months we&#8217;ve been manually scaling up and back down.</p>
<p>Occasionally we&#8217;d scale up and then forget to back off on the dynos (web servers) when traffic went back down. It&#8217;s wasteful to have those extra dynos idle and costs us a lot of money.</p>
<p>Last week we found <a href="http://hirefireapp.com/">HireFire</a>. Which is a service that monitors your application and scales it up or down depending on the performance.</p>
<p>It&#8217;s only been a week and the change in our heroku bill has been HUGE. HireFire is only $10 a month. It&#8217;s saving us far more money than that and frees me up to do other things than monitor heroku.</p>
<p>We&#8217;re running the beta version. Check it out here: <a href="https://github.com/meskyanichi/hirefire-resource"><a href="https://github.com/meskyanichi/hirefire-resource">https://github.com/meskyanichi/hirefire-resource</a></a></p>
