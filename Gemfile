# frozen_string_literal: true

source "https://rubygems.org"
gemspec

group :jekyll_plugins do
  gem 'jekyll-feed'
  gem 'jekyll-paginate'
  gem 'github-pages'
  gem 'jekyll-seo-tag'
  gem 'rake'
end

require 'rbconfig'
  if RbConfig::CONFIG['target_os'] =~ /darwin(1[0-3])/i
    gem 'rb-fsevent', '<= 0.9.4'
  end
