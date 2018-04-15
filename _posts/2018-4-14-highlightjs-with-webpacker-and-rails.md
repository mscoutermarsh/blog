---
layout: post
title: Highlight.js with Webpacker and Rails
---

Key is: use the hightlightjs shim: https://github.com/components/highlightjs

Install using yarn.
```
yarn add highlightjs
```

Inside application.js, add:
```
import hljs from 'highlightjs'
hljs.initHighlightingOnLoad();
```
