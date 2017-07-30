source "https://rubygems.org"

gem 'jekyll'

# If you want to use GitHub Pages, remove the "gem "jekyll"" above and
# uncomment the line below. To upgrade, run `bundle update github-pages`.
# gem "github-pages", group: :jekyll_plugins

# If you have any plugins, put them here!
group :jekyll_plugins do
  gem 'jekyll-feed', '~> 0.6'
  gem 'jekyll-paginate-v2'
  gem 'jekyll-compose'
end

group :test, :development do
  gem 's3_website'
end

# Windows does not include zoneinfo files, so bundle the tzinfo-data gem
gem 'tzinfo-data', platforms: [:mingw, :mswin, :x64_mingw, :jruby]

