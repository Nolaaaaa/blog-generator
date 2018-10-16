---
title: CSS一些零碎知识整理
date: 2018-10-01 22:30:35
tags: CSS
categories: 前端
---
文本溢出省略、内容超出范围部分滚动、清除滚动条、加3D效果等等
<escape><!-- more --></escape>

### 1 图形居中
```css
/* 置一个一个图形在页面居中，中线是左边的那条线 */
/* 如果想以图形的中线居中可以用：margin-right: 图形宽度的一半;也可以用transform: translateX(-50%) */
position: absolute;
left: 50%;
```

### 2 让内容不超出范围，超出部分产生滚动
```css
overflow: auto;
height: 100%;
```

### 3 清除滚动条
```css
overflow-y: hidden;
overflow-x: hidden;
```

### 4 加3D效果
```css
/* 父元素加 */
perspective:2000px; 
/* 子元素加 */
transform:rotateY(25deg);
```
但是这个时候，字体会变模糊。传说中是由于浏览器渲染的问题，宽高不能设置为奇数。

### 5 一段文本超出可视范围时隐藏超出部分并显示…
```css
/* 单行文本溢出省略 */
.textOver{
  white-space:nowrap;
  text-overflow:ellipsis;
  overflow:hidden;    
  word-break: break-all;
}

/* 多行文本溢出省略 */
.textOver(@n:2){
  overflow: hidden;
  text-overflow: ellipsis;
  word-break: break-all;
  display: -webkit-box;
  -webkit-line-clamp: @n;
  -webkit-box-orient: vertical;
};
```

### 6 中文词组自动换行（词组间有空格）
```html
<!-- 第一种 -->
<div style="word-break: keep-all">美白 明目 减脂<div>
<!-- 第二种 -->
<div style="white-space: pre-wrap">
  <span style="display: inline-block">美白</span>
  <span style="display: inline-block">明目</span> 
  <span style="display: inline-block">减脂</span>
<div>
```
`white-space`的其他值
```css
white-space: normal	 /* 默认。空白会被浏览器忽略。 */
white-space: pre	/* 空白会被浏览器保留。其行为方式类似 HTML 中的 <pre> 标签。 */
white-space: nowrap	/* 文本不会换行，文本会在在同一行上继续，直到遇到 <br> 标签为止。 */
white-space: pre-wrap	/* 保留空白符序列，但是正常地进行换行。 */
white-space: pre-line	/* 合并空白符序列，但是保留换行符。 */
white-space: inherit	/* 规定应该从父元素继承 white-space 属性的值。 */
```

`word-break`的其他值
```css
word-break:break-all; /* 强制英文单词断行 */
word-wrap:break-word; /* 自动换行*/
word-break:keep-all; /* 只能在半角空格或连字符处换行 */
```

### 7 去掉点击输入框时的输入框的边框
```css
outline: none;
```

### 8 页面宽度超出屏幕的宽度可左右滑动解决办法
```css
height: 100%;
overflow: hidden
```