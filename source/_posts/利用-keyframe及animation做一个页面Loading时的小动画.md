---
title: 利用@keyframe及animation做一个页面Loading时的小动画
date: 2018-05-08 15:57:39
tags: [CSS]
categories: 前端
---

利用@keyframe规则和animation常用属性做一个页面Loading时的小动画
<escape><!-- more --></escape>
### 1  @keyframe规则简介
1. @keyframes定义关键帧，即动画每一帧执行什么。
要使用关键帧, 先创建一个带名称的@keyframes规则，以便后续使用 animation-name 这个属性来调用指定的@keyframes. 每个@keyframes 规则包含多个关键帧，也就是一段样式块语句，每个关键帧有一个百分比值作为名称，代表在动画进行中，在哪个阶段触发这个帧所包含的样式。
关键帧的编写顺序没有要求，最后只会根据百分比按由小到大的顺序触发。
2. 语法
```css
@keyframes <identifier> {
  [ [ from | to | <百分比> ] [, from | to | <百分比> ]* block ]*
}

<identifier>
帧列表的名称。 名称必须符合 CSS 语法中对标识符的定义。
from
等效于 0%.
to
等效于 100%.

注意⚠️：@keyframes 不能在内联样式中使用
```

### 2  animation常用属性简介
1. animation定义动画每一帧如何执行。
该属性允许配置动画时间、时长以及其他动画细节，但该属性不能配置动画的实际表现，动画的实际表现是由  [@keyframes](https://developer.mozilla.org/zh-CN/docs/Web/CSS/@keyframes) 规则实现。
2. animation的属性
**animation-delay**
设置延时，即从元素加载完成之后到动画序列开始执行的这段时间，单位一般为秒（s）或毫秒（ms），若为负值表示跳过前几秒执行。
**animation-direction**
设置动画在每次运行完后是反向运行还是重新回到开始位置重复运行。
	* normal：默认值，动画按正常播放;
	* reverse：动画反向播放;
	* alternate：动画在奇数次正向播放，在偶数次反向播放；
	* alternate-reverse：动画在奇数次反向播放，在偶数次正向播放；
	* initial：设置该属性为它的默认值；
	* inherit：从父元素继承该属性。
**animation-duration**
设置动画一个周期的时长。
**animation-iteration-count**
设置动画重复次数， 可以指定infinite无限次重复动画
**animation-name**
指定由@keyframes描述的关键帧名称。
**animation-play-state**
允许暂停和恢复动画。
	* paused：指定动画暂停；
	* running：指定动画运行；
**animation-timing-function**
设置动画速度， 即通过建立加速度曲线，设置动画在关键帧之间是如何变化。
**animation-fill-mode**
指定动画执行前后如何为目标元素应用样式。

### 3  实例：一个页面Loading时的小动画
点击查看动画效果[点击](http://js.jirengu.com/yutoyemoja/3/edit?html,css,output)

