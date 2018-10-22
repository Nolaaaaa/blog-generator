---
title: CSS水平布局各栏高度自动相等
date: 2018-10-16 23:08:24
tags: CSS
categories: 前端
---
CSS水平布局各栏高度自动相等
<escape><!-- more --></escape>

### 1 table 实现
[效果预览](http://js.jirengu.com/jayifacara/4/edit)
```css
.container {
  display: table;
  width: 100%
}
.left {
  display: table-cell;
  background: rgb(222, 222, 248);
  width: 20%;
} 
.right {
  display: table-cell;
  background: rgb(248, 204, 204);
  width: 80%;
}
```

### 2 flex 实现
[效果预览](http://js.jirengu.com/banepayohi/1/edit)
```css
.container {
  display: flex;
  width: 100%
}
.left {
  flex: 1;
  background: rgb(222, 222, 248);
  width: 20%;
}
.right {
  background: rgb(248, 204, 204);
  width: 80%;
}

```
### 2 grid 实现
[效果预览](http://js.jirengu.com/yucatezexo/22/edit)
```css
.container {
  display: grid; 
  grid-template-columns: 20% 80%; 
  grid-template-rows: auto; 
}
.left {
  background: rgb(268, 222, 248);
}
.right {
  background: rgb(248, 204, 204);
}
```
PS: 一个关于grid的练习 shttp://js.jirengu.com/zisinokipu/8/edit