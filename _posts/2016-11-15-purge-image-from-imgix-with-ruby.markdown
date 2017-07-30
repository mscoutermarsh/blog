---
layout: post
title: Purge image from imgix with Ruby
date: 2016-11-15 10:31:18.000000000 -08:00
---
Here's how we purge images from imgix's cache at Product Hunt with Ruby.

If you're googling around for a solution, hopefully this code helps you out.

Imgix docs: https://docs.imgix.com/setup/purging-images

```ruby
# usage: Set env variable IMGIX_API_KEY to your api key
# Need to add httparty to your gemfile
#
# Cdn::Purge.cache.call('urlhere')
class Cdn::PurgeCache
  include HTTParty
  API_KEY = ENV.fetch('IMGIX_API_KEY'.freeze)
  base_uri 'https://api.imgix.com/v2'.freeze

  class << self
    def call(url)
      new(url).call
    end
  end

  def initialize(url)
    @options = { body: { url: url }, basic_auth: { username: API_KEY, password: '' } }
  end

  def call
    purge = self.class.post('/image/purger'.freeze, @options)
    purge.success?
  rescue Errno::ECONNRESET, SocketError
    { 'status' => 'failed' }
  end
end
```
