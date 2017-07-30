---
layout: post
title: RSpec for sitemap_generator
date: 2015-05-29 12:25:50.000000000 -07:00
---
If you use [sitemap_generator](https://github.com/kjvarga/sitemap_generator), you have probably accidently broken it a couple times as well. It tends to be a blindspot in tests because it isn't exactly easy to write tests for.

Here is a super simple rspec test you can use to at the very least, ensure that it runs. This will protect you from changing a route & forgetting to update it in your sitemap config.

```ruby
# spec/lib/sitemap_generator/interpreter_spec.rb
require 'spec_helper'

describe SitemapGenerator::Interpreter do
  describe '.run' do
    it 'does not raise an error' do
      allow(SitemapGenerator::Sitemap).to receive(:ping_search_engines).and_return true
      allow(SitemapGenerator::Sitemap).to receive(:create).and_yield

      # Create some test data here with FactoryGirl

      expect { described_class.run }.not_to raise_error
    end
  end
end

```

