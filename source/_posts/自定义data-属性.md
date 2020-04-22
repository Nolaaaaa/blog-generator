---
title: 自定义data-*属性
date: 2018-08-09 17:19:26
tags: [HTML/DOM]
categories: 前端
---

自定义data-*属性，HTML标签上添加以 “data-“开头的属性即可
<escape><!-- more --></escape>
```html
//HTML中
<li id="tab" data-role="tabbbb" data-view="#rec-view" >推荐</li>
```
读取的时候是通过dataset对象，使用”.”来获取属性，需要去掉data-前缀，连字符需要转化为驼峰命名
```javascript
//js中读取
let tab = document.querySelector('#tab')
tab.dataset.role
```