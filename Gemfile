source "https://rubygems.org"

ruby ">4.0"

gem "rake"

gem "tzinfo-data"
gem "wdm", "~> 0.1.0" if Gem.win_platform?

# github-pages >= 228 cannot install on Ruby 4 (commonmarker requires < 4.0).
# Use the same Jekyll/Liquid versions GitHub Pages runs (232).
group :jekyll_plugins do
  gem "jekyll", "~> 3.10.0"
  gem "jekyll-paginate"
  gem "jekyll-sitemap"
  gem "jekyll-gist"
  gem "jekyll-feed"
  gem "jemoji"
  gem "jekyll-include-cache"
  gem "jekyll-redirect-from"
  gem "jekyll-remote-theme"
  gem "jekyll-seo-tag"
  gem "jekyll-algolia"
end

gem "kramdown-parser-gfm", "~> 1.1"

# Extracted from default gems in Ruby 3.4+/4.0
gem "csv"
gem "logger"
gem "base64"
gem "bigdecimal"
gem "mutex_m"
gem "ostruct"
gem "faraday-retry"
