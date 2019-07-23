---
title: JS常用函数整理
date: 2019-07-23 19:48:58
tags: JavaScript
categories: 前端
---

一些JS常用函数整理，如防抖、节流等函数，以便以后工作遇到相关问题时提高工作效率
<escape><!-- more --></escape>


#### 防抖
防抖查看效果：[地址](https://jsbin.com/religexeka/61/edit?html,js,output)
```JS
// 触发间隔超过指定间隔的任务才会执行，连续发生动作会刷新时间
function debounce(fn, delay, ...args) {
  let timer = null
  return function () {
    clearTimeout(timer)
    timer = setTimeout(() => {
      fn.apply(this, args)
    }, delay)
  }
}
```

#### 节流
节流查看效果：[地址](https://jsbin.com/religexeka/61/edit?html,js,output)
```JS
// 在指定间隔内任务只执行一次，连续发生的动作会被忽略
function throttle(fn, delay, ...args) {
  let prev = Date.now()
  return function () {
    let now = Date.now()
    if (now - prev >= delay) {
      fn.apply(this, args)
      prev = Date.now()
    }
  }
}

function throttle(fn, delay, ...args) {
  let timer
  return function () {
    if (!timer) {
      timer = null
      timer = setTimeout(() => {
        timer = null
        fn.apply(this, args)
      }, delay)
    }
  }
}
```