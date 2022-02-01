---
layout: post
title: Using GitHub Package Registry with Netlify
date: 2022-02-01 16:52 -0500
---

Here's the best way I've found to get GitHub Packages working with a Netlify build.


1. Add this file to your repo. I put it at `.npmrc-netlify` in the root of the project. 

This keeps it from interfering with any `.npmrc` you might already have and need for local development.

```
@planetscale:registry=https://npm.pkg.github.com
//npm.pkg.github.com/:_authToken=${GITHUB_PACKAGES_KEY}
legacy-peer-deps=true
```

2. Next, in your `netlify.toml`, add this:

```
[build.environment]
  NPM_CONFIG_USERCONFIG = ".npmrc-netlify"
```

3. Then last step is setting the `GITHUB_PACKAGES_KEY` in your projects environment variables.
