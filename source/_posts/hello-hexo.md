---
title: Hello, Hexo
categories:
date: 01-12-2016
tags:
---

It has been quite a while since I last wrote a blog post. I used to have a blog on [WordPress](https://babelthuap.wordpress.com/), but hey -- *I know code now*. And I have more interesting things to say! For instance, I'm pretty proud of this one-line [memoized](https://en.wikipedia.org/wiki/Memoization) Fibonacci function:

```javascript
let m = {0: 1, 1: 1}, fib = n => m[n] ? m[n] : m[n] = fib(n - 1) + fib(n - 2);
```

Pretty nifty.

