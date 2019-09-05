---
title: CSS使用小技巧整理
date: 2019-07-23 19:59:25
tags: CSS
categories: 前端
---

一些CSS使用的小技巧整理，如实现文字的两端对齐等
<escape><!-- more --></escape>

1. 改变光标的颜色：`caret-color`
2. 对背景进行文本裁剪：`background-clip` [MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/background-clip)
3. 改变选择的文本颜色：`::selection`
4. 文字倒序：`letter-spacing`（值为负数且为字号的两倍）
5. 校验表单输入的内容：`:valid`、`:invalid`配合`pattern`[效果](https://codepen.io/JowayYoung/pen/QemxKr)
6. 使图片自适应：`object-fit`（类似小程序的mode）[MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/object-fit)
7. 调整文本排版方向：`writing-mode`
8. 修改滚动条样式：`scrollbar`[MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/::-webkit-scrollbar)
9. 利用伪元素绘制各种图形：[网站地址](https://css-tricks.com/the-shapes-of-css/)

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
PS：`text-align-last: justify;`也可实现同样的功能 [效果](https://jsbin.com/kijekolulo/2/edit?html,css,output))

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
### 使用vw定制rem自适应布局
```CSS
html {
  font-size: calc(100vw / 7.5);
}
```

### 文本溢出省略
```LESS
// 多行
.test {
  overflow: hidden;
  text-overflow: ellipsis;
  word-break: break-all;
  display: -webkit-box;
  -webkit-line-clamp: @n;   // 值可根据行数修改
  -webkit-box-orient: vertical;
}
// 一行
.test {
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;   
  word-break: break-all;
}
```

### 利用linear-gradient使背景颜色动态渐变
[效果](https://jsbin.com/jikinuminu/13/edit?html,css,output)
```CSS
.test {
  background: linear-gradient(135deg, #FF4040, #FFA500, #ADFF2F, #40E0D0, #BF3EFF) left center/400% 400%;
  animation: move 10s infinite;
}

@keyframes move {
	0%,
	100% {
		background-position-x: left;
	}
	50% {
		background-position-x: right;
	}
}
```

### attr()获取DOM元素上的属性值
[效果](https://jsbin.com/sicofafexe/edit?html,css,output)
```CSS
div::after {
  content: attr(test);
}
```

