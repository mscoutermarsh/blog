---
layout: post
title: Install Java during build - Cloud66
date: 2013-04-27 09:28:08.000000000 -07:00
---
If your Rails app requires Java and you're using <a href="http://cloud66.com">Cloud66</a>, you'll want to setup a script to automatically install it on your web servers during the build process.

You can do this with <a href="https://www.cloud66.com/help/deploy_hooks">deployment hooks</a>. These let you specify shell scripts to run during the build process of your servers.

Below I have an example of a shell script for installing Java. I've also included an example of what to add to your deploy_hooks.yml.

**deploy_hooks.yml**
```yml
production:
    first_thing:
      - source: /.cloud66/files/java.sh
        destination: ~/java.sh
        target: rails
        sudo: true
        execute: true
        apply_during: build_only
        halt_on_error: true
```

**java.sh**
```shell
sudo apt-get install -y openjdk-7-jdk
```
