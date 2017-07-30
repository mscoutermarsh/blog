---
layout: post
title: Grape/Rails API Cross-Origin Resource Sharing
date: 2012-12-16 09:42:00.000000000 -08:00
---
If you want to allow access to your <a href="https://github.com/intridea/grape">Grape API</a> from other domains, you need to implement CORS!

It’s easy with the<a href="https://github.com/cyu/rack-cors"> rack-cors gem</a>. Here’s how you do it.

Add rack-cors to your gemfile.
<pre>gem 'rack-cors', :require =&gt; 'rack/cors'</pre>
Then go to your application.rb and add:
<div class="gist"><a href="https://gist.github.com/4297940">[gist id="4297940"]</a></div>
<div class="gist"></div>
<div class="gist"><a href="https://gist.github.com/4297940">https://gist.github.com/4297940</a></div>
done!
