---
layout: post
title: 'Introducing: The Taskk API'
date: 2013-01-01 02:06:30.000000000 -08:00
---
Each week we get multiple emails from taskk users that go something like this...
<blockquote>Taskk must integrate with X! Please! do it! please!</blockquote>
So, about 3 months ago, I started working on the <a href="https://api.taskk.it">Taskk API</a>. And today, it's live!<a href="https://api.taskk.it">
</a>

Even though I've written a couple of APIs before. I came away from this project learning a lot more than I expected to.

<a href="https://api.taskk.it"><img class=" alignnone" alt="taskk api" src="https://dl.dropbox.com/u/18216283/blog/taskk_api.jpg" width="359" height="314" /></a>
<h2>An API built with Grapes!</h2>
<a href="https://taskk.it">Taskk</a> is built with Ruby on Rails. It is possible to build an API using Rails controllers, but it's cumbersome and there are better ways to go about it.

We decided on using <a href="https://github.com/intridea/grape">Grape</a>. Which in addition to being delicious... it is also a micro-framework that does one thing and does it really well. It builds APIs.

I've written about Grape a few times:
<ul>
	<li><a title="Rails + Grape + API Key Authentication" href="http://mikecoutermarsh.com/2012/10/24/rails-grape-api-key-authentication/">Rails + Grape + API Key Authentication</a></li>
	<li><a title="Grape/Rails API Cross-Origin Resource Sharing" href="http://mikecoutermarsh.com/2012/12/16/grape-cors/">Grape/Rails API Cross-Origin Resource Sharing</a></li>
	<li><a title="Organize your Grape API (in Rails)" href="http://mikecoutermarsh.com/2012/11/27/organize-your-grape-api-in-rails/">Organize your Grape API (in Rails)</a></li>
</ul>
If you're familiar with <a href="http://www.sinatrarb.com/">Sinatra</a>, Grape is similar. But comes with all of the helper methods you need for building an API.

Overall, my experience with it was really good. I'd use it again. If you're planning on writing your own API, you should give it a serious look

&nbsp;
<h2>Authentication</h2>
When developing an API you have a few options for authenticating users. Each has its own level of security and challenges in implementation.

We needed our API to be both secure and easy to use.

We chose to authenticate with API keys. We issue each user an API token. They then append this unique token to each API request and it authenticates them. We went with this method because it is relatively simple to implement and keeps users credentials entirely out of the process. Our users also have the ability to revoke and reissue their keys through their settings panel.

You can see how I implemented API keys here:<a title="Rails + Grape + API Key Authentication" href="http://mikecoutermarsh.com/2012/10/24/rails-grape-api-key-authentication/" target="_blank"> Rails + Grape + API Key Authentication</a>.

We make this more secure by forcing all API requests to go over HTTPS. If you're using heroku,<a title="Adding SSL to Heroku with Cloudflare" href="http://mikecoutermarsh.com/2012/12/24/adding-ssl-to-heroku-with-cloudflare/"> setting up SSL is really easy</a>.
<h2>Planning</h2>
Implementing the API was the easy part. Planning and documenting how it should work was the challenge.

Once you release an API. It can't change. Once other people are using it, making changes <strong>will break their apps</strong>. So you really need to get it right the first time (or have a great versioning system, <a href="https://github.com/intridea/grape#versioning">like Grape gives you</a>).

We started the process with a discussion on what data we wanted to expose. We kept iterating over the list until we had a pretty good idea of what we needed.

Then once we had that together and I had implemented it, we started using it internally when building out new features. I'm glad we did this because it showed us functionality that it really needed and I missed during my first pass.
<h2>Documentation</h2>
50% of your effort when building an API is the documentation. It needs to be<em> really</em> good, readable and accurate. There are some frameworks out there that write it for you (such as <a href="https://github.com/wordnik/swagger-ui">Swagger</a>). I played around with those, but found that writing it on my own was best. One of the side effects of writing your own documentation is that it becomes really apparent where there are opportunities for improvement.
<h2>Testing</h2>
I used <a href="http://rspec.info/">RSpec</a> for testing. RSpec and Rails make testing an API really easy.

Here's an example of a test. This is insuring that a valid token is needed to call PING.
<pre>describe "GET #{api_domain}/auth/ping" do
  it "should return unauthorized" do
    get "#{api_domain}/auth/ping?token=abcdefg"
    response.status.should == 401
    JSON.parse(response.body)['error'].should == "Unauthorized. Invalid or expired token."
  end
end</pre>
In addition to the usual benefits of having robust specifications, I found that they helped me in writing the documentation because I knew exactly what to expect from the API.
<h2>Build with it!</h2>
Finally, the best part of all is that we are now starting to use it to build more features and integrate <a href="https://taskk.it">Taskk</a> with other services.

It will be the backbone for our soon-to-be iPhone app and will be the launching pad for all the features our users have been requesting.

<a href="https://api.taskk.it">Check it out!</a>

--

I'm friendly, Get in touch! Shoot me an <a href="mailto:coutermarsh.mike@gmail.com">email</a> or <a href="http://twitter.com/mscccc">@mscccc</a>.
