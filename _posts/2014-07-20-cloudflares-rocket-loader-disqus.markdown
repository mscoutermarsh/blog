---
layout: post
title: Cloudflare's Rocket Loader + Disqus
date: 2014-07-20 07:33:41.000000000 -07:00
---
Cloudflare's [Rocket Loader](https://support.cloudflare.com/hc/en-us/articles/200168056-What-does-Rocket-Loader-do-) speeds up page rendering by asynchronously loading all of your javascript. When in **automatic** mode, Rocket Loader will do this to all scripts on the page.

I've found that this often breaks the loading of [Disqus](https://disqus.com/) comments.

We can fix this by selectively telling Rocket Loader which scripts to skip by adding a data attribute to the script tag.

A standard Disqus installation looks like this:
```html
  <div id="disqus_thread"></div>
  <script type="text/javascript">
      var disqus_shortname = 'example'; // required: replace example with your shortname
      var disqus_identifier = 'example';
      (function() {
          var dsq = document.createElement('script'); dsq.type = 'text/javascript'; dsq.async = true;
          dsq.src = '//' + disqus_shortname + '.disqus.com/embed.js';
          (document.getElementsByTagName('head')[0] || document.getElementsByTagName('body')[0]).appendChild(dsq);
      })();
  </script>
  <noscript>Please enable JavaScript to view the <a href="http://disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript>
```

We can make it compatible with CloudFlare by adding ```data-cfasync="false"``` to the script tag.

Resulting in a script that looks like this:
```html
  <div id="disqus_thread"></div>
  <script data-cfasync="false" type="text/javascript">
      var disqus_shortname = 'example'; // required: replace example with your shortname
      var disqus_identifier = 'example';
      (function() {
          var dsq = document.createElement('script'); dsq.type = 'text/javascript'; dsq.async = true;
          dsq.src = '//' + disqus_shortname + '.disqus.com/embed.js';
          (document.getElementsByTagName('head')[0] || document.getElementsByTagName('body')[0]).appendChild(dsq);
      })();
  </script>
  <noscript>Please enable JavaScript to view the <a href="http://disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript>
```

Once this change is complete, make sure to [purge your cache](https://support.cloudflare.com/hc/en-us/articles/200169246-How-do-I-purge-my-cache-) and you'll be good to go.
