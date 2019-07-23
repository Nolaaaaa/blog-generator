---
title: CSS使用小技巧整理
date: 2019-07-23 19:59:25
tags: CSS
categories: 前端
---

一些CSS使用的小技巧整理，如实现文字的两端对齐等
<escape><!-- more --></escape>


### CSS 实现文字的两端对齐
[效果](https://jsbin.com/jukapifido/edit?html,css,output)
```css
.test {
  width: 100px;
  text-align:justify;
}
.test:after{
  content:"";
  display: inline-block;
  width:100%;
  overflow:hidden;
  height:0;
}
```