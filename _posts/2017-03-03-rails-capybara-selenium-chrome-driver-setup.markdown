---
layout: post
title: Rails + Capybara + Selenium + Chrome Driver setup
date: 2017-03-03 10:22:54.000000000 -08:00
---
Just setup our Capybara tests to use Chrome driver.

Here is how I did it.

First, added

```ruby
group :test do
  gem 'capybara'
  gem 'selenium-webdriver'
  gem 'chromedriver-helper' # <- New!
end
```

Then in the spec_helper.rb

```ruby
Capybara.register_driver :selenium do |app|
  Capybara::Selenium::Driver.new(app, browser: :chrome)
end

Capybara.javascript_driver = :chrome

Capybara.configure do |config|
  config.default_max_wait_time = 10 # seconds
  config.default_driver        = :selenium
end
```

Finally, we had an issue with the chrome driver running too small of a window, cutting off some content in our tests. To fix that, I added this to make the window huge. (you may not need this)

```ruby
RSpec.configure do |config|
  # ...
  config.before(:each, type: :feature) do
    # Note (Mike Coutermarsh): Make browser huge so that no content is hidden during tests
    Capybara.current_session.driver.browser.manage.window.resize_to(2_500, 2_500)
  end
```

Hope this helps if you're setting up the same!
