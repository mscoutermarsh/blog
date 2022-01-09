---
layout: post
title: Running queries in parallel with Rails 7 and load_async
date: 2022-01-08 11:31 -0500
---
Rails 7 has added the [`load_async`](https://edgeapi.rubyonrails.org/classes/ActiveRecord/Relation.html#method-i-load_async) method to ActiveRecord. This looks like it could be a huge performance win for many Rails applications.

Normally, a Rails app will execute all of its queries serially (one after another). The execution of the request waits while the query is executed. With `load_async`, you can now send your queries to the background and let them execute while your foreground thread continues on with the request.

If you're familiar with using something like `Promise.all` to execute several queries in a Node.js app at once. This works out to be fairly similar.

## Example
I set up an intentionally slow example app that executes 4 queries to render a page. To simulate some latency, the app uses a [PlanetScale database](https://planetscale.com) in Europe (I'm in the US) to add a trip across the ocean for each query.

Each table has thousands of records and the page renders a partial for each, so it has plenty to do while the queries are running.

### Without async
Each query gets run once the view starts to render. The page stops rendering and waits for queries to run 4 different times.

```ruby
def index
  @posts = Post.all
  @users = User.all
  @organizations = Organization.all
  @tweets = Tweet.all
end
```

**Result:**
Page render time: 1095ms

```ruby
Post Load (104.2ms)  SELECT `posts`.* FROM `posts`
↳ app/views/posts/index.html.erb:6

Organization Load (197.2ms)  SELECT `organizations`.* FROM `organizations`
↳ app/views/posts/index.html.erb:17

Tweet Load (206.5ms)  SELECT `tweets`.* FROM `tweets`
↳ app/views/posts/index.html.erb:28

User Load (101.0ms)  SELECT `users`.* FROM `users`
↳ app/views/posts/index.html.erb:39

```

### With load_async
With `load_async` added to the queries, we now give Rails the option to them all at once in separate threads. You'll see in the logs that 3 of the 4 queries are run async. The `posts` query gets rendered by the view before the query has time to run, so Rails runs it in the foreground since it's needed immediately.

```ruby
def index_async
  @posts = Post.all.load_async
  @users = User.all.load_async
  @organizations = Organization.all.load_async
  @tweets = Tweet.all.load_async
end
```

**Result:**
709ms

```ruby
Post Load (104.9ms)  SELECT `posts`.* FROM `posts`
↳ app/views/posts/index_async.html.erb:6

ASYNC Organization Load (169.6ms) (db time 193.4ms)  SELECT `organizations`.* FROM `organizations`
↳ app/views/posts/index_async.html.erb:17

ASYNC Tweet Load (24.4ms) (db time 278.0ms)  SELECT `tweets`.* FROM `tweets`
↳ app/views/posts/index_async.html.erb:28

ASYNC User Load (0.0ms) (db time 204.8ms)  SELECT `users`.* FROM `users`
↳ app/views/posts/index_async.html.erb:39
```

### 35% Faster

For our example app, this one page sped up by 35%. Pretty impressive results. This is a contrived example of course with a small number of very slow queries. Most production Rails apps that I've worked on generally execute anywhere from 20 to hundreds of individual queries per page. It will be interesting to see how this can be used to improve the performance of more complex applications.


## Setup

To do this in your own app you need to be on Rails 7 and make a configuration page.

In your config files, you'll need to set the `async_query_executor` setting. Otherwise `load_async` will be a no-op.

I set it to:

```ruby
# config/environments/development + production.rb files
config.active_record.async_query_executor = :global_thread_pool
```

If you use multiple databases in your application, then you can use `multi_thread_pool` to establish a pool per database.

```ruby
config.active_record.async_query_executor = :multi_thread_pool
```

## Connection Limits

Generally, Rails apps establish a database connection pool size based on the number of threads the application is running.

Now that you can run multiple queries at once, you need to be careful to give your application enough connections to execute queries in the background. You should update the `pool_size` in your database.yml to account for this.

By default the `global_thread_pool` setting will use a maximum of 4 connections at a time. This can be changed by setting `config.active_record.global_executor_concurrency`.

## More reading
- [Load async docs](https://edgeapi.rubyonrails.org/classes/ActiveRecord/Relation.html#method-i-load_async)
- [async_query_executor config docs](https://edgeguides.rubyonrails.org/configuring.html#config-active-record-async-query-executor)
- [Original pull request](https://github.com/rails/rails/pull/40037)
- [Thread pool pull request](https://github.com/rails/rails/pull/41495)
