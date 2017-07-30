---
layout: post
title: dotfiles - faster commit messages
date: 2013-10-05 21:21:17.000000000 -07:00
---
I use <a href="http://zachholman.com/2010/08/dotfiles-are-meant-to-be-forked/">dotfiles</a> to speed up my workflow. I recently threw together this script to speed up writing my git commit messages.

It auto adds the branch name to your commits.

**So, instead of this:**
```ruby
git commit -m "[name of ticket] here is my commit message"
```
**I can now type this:**
```ruby
git-commit "here is my commit message"
```
This is awesome because now I can spend time doing things other than typing.

Learn more about <a href="http://zachholman.com/2010/08/dotfiles-are-meant-to-be-forked/">dotfiles here</a>.

Here's the code:

```ruby
#!/usr/bin/env ruby

# git-commit. Script for speeding up writing commit msg's.
# Adds branch name to beginning of commit message if on any branch other than master.

# Options:
# -s for appending [ci skip] to msg

branch = `git rev-parse --abbrev-ref HEAD`.gsub("\n",'')
skip, commit_msg = ""

if ARGV[0] == '-s'
  ARGV.shift
  skip = " [ci skip]"
end

branch = (branch == "master") ? "" : "[#{branch}] "
commit_msg = "#{branch}#{ARGV.join('')}#{skip}"

command = "git commit -m \"#{commit_msg}\""

puts command
exec command
```
