source "https://rubygems.org"

# Use the Jekyll and plugin versions supported by GitHub Pages.
gem "github-pages", group: :jekyll_plugins

# Required when serving Jekyll locally with newer Ruby versions.
gem "webrick", "~> 1.8"

# Windows and JRuby do not include zoneinfo files.
platforms :mingw, :x64_mingw, :mswin, :jruby do
  gem "tzinfo", ">= 1", "< 3"
  gem "tzinfo-data"
end

# Improves file watching when running Jekyll locally on Windows.
gem "wdm", "~> 0.1", platforms: [:mingw, :x64_mingw, :mswin]

# Required for JRuby compatibility.
gem "http_parser.rb", "~> 0.6.0", platforms: [:jruby]