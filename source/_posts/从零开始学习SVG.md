---
title: 从零开始学习SVG
date: 2018-06-01 20:13:50
tags: SVG
categories: 前端
---

SVG是一种用来描述二维矢量图形的标记语言
<escape><!-- more --></escape>
### 1  什么是SVG？
1. MDN中的定义是：SVG即可缩放矢量图形**（Scalable Vector Graphics，SVG)**，是一种用来描述二维矢量图形的  [XML](https://developer.mozilla.org/zh-CN/docs/XML)  标记语言。 简单地说，SVG 面向图形，HTML 面向文本。
2. * SVG 与 Flash 类似，都是用于二维矢量图形，二者的区别在于，SVG 是一个  [W3C 标准](http://www.w3.org/Graphics/SVG/) ，基于 XML，是开放的，而 Flash 是封闭的基于二进制格式的。因为都是 W3C 标准，SVG 与其他的  [W3C](http://www.w3.org/)  标准，比如  [CSS](https://developer.mozilla.org/zh-CN/docs/CSS) 、 [DOM](https://developer.mozilla.org/zh-CN/docs/DOM)  和  [SMIL](http://www.w3.org/AudioVideo/)  等能够协同工作。

### 2  SVG的坐标系统
对于所有元素，SVG使用的坐标系统或者说网格系统，和 [Canvas](https://developer.mozilla.org/zh-CN/HTML/Canvas) 用的差不多（所有计算机绘图都差不多）。这种坐标系统是：以页面的左上角为(0,0)坐标点，坐标以像素为单位，x轴正方向是向右，y轴正方向是向下。注意，这和你小时候所教的绘图方式是相反的。但是在HTML文档中，元素都是用这种方式定位的。
[SVG](https://i.loli.net/2018/08/17/5b767635aeb73.png)

### 3  画图形
1. 画矩形（rect）
```html
<rect x="60" y="10" rx="10" ry="10" width="30" height="30"/>
/*

x        矩形左上角的x位置
y        矩形左上角的y位置
width    矩形的宽度
height   矩形的高度
rx       圆角的x方位的半径 
ry       圆角的y方位的半径
*/
```
2. 画圆形（circle）
```html
<circle cx="25" cy="75" r="20"/>
/*
r   圆的半径
cx  圆心的x位置
cy  圆心的y位置
*/
```
3. 画椭圆（ellipse）
```html
<ellipse cx="75" cy="75" rx="20" ry="5"/>
/*
rx  椭圆的x半径
ry  椭圆的y半径
cx  椭圆中心的x位置
cy  椭圆中心的y位置
*/
```
4. 画直线（line）
```html
<line x1=“10” x2=“50” y1=“110” y2=“150”/>
/*
x1   起点的x位置
y1   起点的y位置
x2   终点的x位置
y2   终点的y位置
*/
```
5. 画折线（polyline）
```html
<polyline points="60 110, 65 120, 70 115, 75 130, 80 125, 85 140, 90 135, 95 150, 100 145"/>
/*
points
点集数列。每个数字用空白、逗号、终止命令符或者换行符分隔开。每个点必须包含2个数字，一个是x坐标，一个是y坐标。所以点列表 (0,0), (1,1) 和(2,2)可以写成这样：“0 0, 1 1, 2 2”。
*/
```
6. 画多边形（polygon）
```html
<polygon points="50 160, 55 180, 70 180, 60 190, 65 205, 50 195, 35 205, 40 190, 30 180, 45 180"/>
/*
points
点集数列。每个数字用空白符、逗号、终止命令或者换行符分隔开。每个点必须包含2个数字，一个是x坐标，一个是y坐标。所以点列表 (0,0), (1,1) 和(2,2)可以写成这样：“0 0, 1 1, 2 2”。路径绘制完后闭合图形，所以最终的直线将从位置(2,2)连接到位置(0,0)。
*/
```
7. 画路径（path）
```html
<path d="M 20 230 Q 40 205, 50 230 T 90230"/>
```
![path](https://i.loli.net/2018/08/17/5b7677eeeda9f.png)

