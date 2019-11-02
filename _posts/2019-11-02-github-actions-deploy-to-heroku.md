---
layout: post
title: GitHub Actions - Deploy to Heroku
date: 2019-11-02 11:55 -0700
---
Here's how to deploy to Heroku using GitHub Actions. Without adding any new dependencies.

First, you need a Heroku auth token. You can generate one on your machine using the following:

```
heroku authorizations:create
```

Copy the `Token` value and add it as a secret to your repository (Go to your repository Settings -> Secrets. Name it `HEROKU_API_TOKEN`)

Next, add this to your workflow.

```yml
- name: Deploy to Heroku
  env:
    HEROKU_API_TOKEN: ${{ secrets.HEROKU_API_TOKEN }}
    HEROKU_APP_NAME: "your-app-name-here"
  if: github.ref == 'refs/heads/master' && job.status == 'success'
  run: git push https://heroku:$HEROKU_API_TOKEN@git.heroku.com/$HEROKU_APP_NAME.git origin/master:master
```

This will only run for builds on the Master branch and if previous steps have worked properly.

Here's a full example for Rails app. This runs your tests, and if successful, deploys to Heroku.

```yml
name: Ruby Test and Deploy

on: [push]

jobs:
  build:

    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v1
    - name: Set up Ruby
      uses: actions/setup-ruby@v1
      with:
        ruby-version: 2.6.3

    - uses: actions/cache@preview
      with:
        path: vendor/bundle
        key: ${{ runner.os }}-gem-${{ hashFiles('**/Gemfile.lock') }}
        restore-keys: |
          ${{ runner.os }}-gem-

    - name: Install Bundler
      run: |
        gem install bundler

    - name: Install Gems
      run: bundle install --path vendor/bundle --jobs 4 --retry 3

    - name: Run Tests
      run: |
        bundle exec rake test

    - name: Deploy to Heroku
      env:
        HEROKU_API_TOKEN: ${{ secrets.HEROKU_API_TOKEN }}
        HEROKU_APP_NAME: "your-app-name-here"
      if: github.ref == 'refs/heads/master' && job.status == 'success'
      run: git push https://heroku:$HEROKU_API_TOKEN@git.heroku.com/$HEROKU_APP_NAME.git origin/master:master
```
