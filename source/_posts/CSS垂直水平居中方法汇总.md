---
title: CSS垂直水平居中方法汇总
date: 2018-08-02 16:04:05
tags:
---

用思维导图整理了CSS垂直水平居中的一些方法
<escape><!-- more --></escape>
[垂直居中思维导图](https://i.loli.net/2018/08/20/5b7a7d19547b4.png)

### 1  垂直居中
1. 父元素高度确定的单行文本:`height （元素高度）= line-height（行高）`
2. 父元素高度确定的多行文本:
	* 给子元素的`position:relative; top: 50%; translateY(-50%)`
	*  父元素添加伪元素:before，设置伪元素`height: 100%; display: inline-block; vertical-align:middle; `使得子元素实现垂直居中。
	* 插入  table （插入方法和水平居中一样），然后设置  `vertical-align:middle`
	* 先设置 ` display:table-cell`  再设置  `vertical-align:middle`
3. 父元素高度不确定:父元素设置 `position:relative`，子元素设置  `position:absolute ; top: 50%; translateY(-50%)`
4. 万能的`display: flex;`

### 2  水平居中
1. 内联元素居中： 给父元素设置 `text-align:center`
2. 块级元素居中： 
	* 定宽块级元素居中：左右  margin  值设为  auto（前提是已经为元素设置了适当的 width 宽度，否则块级元素的宽度会被拉伸为父级容器的宽度）
	*  不定宽块级元素居中：
		* 给该元素设置  display:inline，使其变成内联元素然后在父元素中设置 text-align：center
		* 给该元素设置  display:inline-block，然后给父元素设置 text-align：center。（加了inline-block，下面会出现一个空隙，如果比较介意这个空隙，可以加vertical-align: top; 消除）
		* 给父元素设置 position:relative，子元素设置  position:absolute ; left: 50%;  margin-left: weight一半值的负数;
		* 将要显示的元素加入到 table 标签当中，然后为 table 标签设置“左右margin”值为“auto”来实现居中。（为什么加入table标签? 是利用table标签的长度自适应性---即不定义其长度也不默认父元素body的长度（table其长度根据其内文本长度决定），因此可以看做一个定宽度块元素，然后再利用定宽度块状居中的margin的方法，使其水平居中。）
3. 万能的display: flex;

### 3  页面垂直水平居中
1. 让子元素在页面居中，可设置父元素：先设置display: flex;然后 justify-content: center;此时子元素左右居中，再align-items: center; 垂直居中。
2. 给父元素设置 position:relative，子元素设置  position: absolute; top: 50%; left: 50%，最后使用负向 margin 实现水平和垂直居中。
	* 如果宽高固定：margin 的值为宽高（具体的宽高需要根据实际情况计算 padding）的一半的负数
	* 如果宽高不固定：transform: translate(-50%, -50%);
