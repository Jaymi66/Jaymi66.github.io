---
title: iPhone 网页中手机号码显示样式问题。
date: 2016-06-26 22:05:26
tags: [web, iPhone]
---

我们在做 iPhone 上的网页的时候会遇到手机号码莫名显示为蓝色的问题,
这是 iPhone 上的 safari 内核的浏览器会自动为手机号码添加 link 以及 link 的样式，
如果这是我们不需要的，该如何禁用掉呢？

<!--more-->

我们可以添加 meta 标签来禁用掉手机号码自动添加link的功能

```html
<meta name="format-detection" content="telephone=no">
```