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

### 实现一个等宽高的正方形
[效果](https://jsbin.com/wilamovuhu/edit?html,css,console,output)
```CSS
div {
  width: 100px;
  background: red;
  display: flex;
}
div::after {
  content: '';
  padding-top: 100%;
}
```

### 快速实现垂直居中
[效果](https://jsbin.com/wilamovuhu/11/edit?html,css,console,output)
```CSS
body {
  min-height: 100vh;
  display: flex;
}
div {
  height: 100px;
  width: 100px;
  margin: auto;
}
```

### 渐变实现进度条
[效果](https://jsbin.com/wilamovuhu/edit?html,css,output)
```CSS
/* 自定义变量：var(--name)   变量名：需 -- 开头*/
div {
  --c: red; 
  --l: 10%;
  height: 10px;
  border: 1px solid;
  border-radius: 5px;
  background: linear-gradient(var(--c), var(--c)) no-repeat;
  background-size: calc(var(--l));
}

```

### 鼠标放元素上时元素自动转圈圈
[效果](https://jsbin.com/wilamovuhu/edit?html,css,output)
```CSS
div {
  height: 100px;
  width: 100px;
  background: red;
}
div:hover {
  transform: rotate(10turn);
  transition: all 10s;
}
```
