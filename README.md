# lindsphotography.github.io

[Personal website](http://lindsaygarciaphotography.com/) of Lindsay Garcia (created by [Stephanie Ortiz](http://stephthedev.com/))

## Prerequisites
1. `bundle install`

## Run locally
1. `bundle exec jekyll serve`
2. `open http://localhost:4000`

## Load contentful data
1. `export CONTENTFUL_SPACE_ID=<some value from contentful>`
2. `export CONTENTFUL_ACCESS_TOKEN=<some value from contentful>`
1. `bundle exec rake contentful:all`
