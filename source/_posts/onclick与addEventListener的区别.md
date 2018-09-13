---
title: onclick与addEventListener的区别
date: 2018-06-01 15:25:35
tags: DOM事件
categories: 前端
---

通过onclick与addEventListener及两个实例子深度解析DOM的冒泡及捕获事件
<escape><!-- more --></escape>
### 1  实例1—点击别处关闭浮层
代码效果演示： [原生JS](http://js.jirengu.com/voqayujovi/1/edit?html,css,js,output)  、  [jQuery](http://js.jirengu.com/redaxabamu/1/edit?html,css,js,output) 
### 2  实例2—点击后颜色一层一个层出现的漂亮的彩虹圈
代码效果： [彩虹圈](http://js.jirengu.com/havaruyele/1/edit?html,css,js,output) 
### 3  onclick与addEventListener的区别
实例1使用的原生JS，为什么使用addEventListener()，而不使用onclick() —onclick()只能添加一个事件，多个事件时只会输出最后一个，而实例1中存在多个事件，不能用onclick()
onclick与addEventListener实际上可分为：Inline events与Event Listeners
####  Event Listeners (“addEventListener” and “IE’s attachEvent”)
两者相同点：都是时间监听器。
两者不同点：
* addEventListener：很多浏览器支持addEventListener(IE9、IE10、IE11、chrome、firefox、opera、safari支持)，使用方式如下：
```javascript
//addEventListener接受三个参数，最后一个参数默认是false。（false表示事件处理将在冒泡阶段执行，true表示事件处理将在捕获阶段执行）
//addEventListener(type,listener,options) 
var target = document.getElementById("test");
target.addEventListener('click',function test(){
        console.log("Hi");
},false)
```
* attachEvent：IE中提供的类似addEventListener的事件监听器，使用方式如下：
```javascript
//qqq
var target = document.getElementById("test");
target.attachEvent('onclick',test);
function test(){
     console.log("Hi");
}		
```
* 理论上，Event Listeners (addEventListener and IE’s attachEvent)可以无限增加事件监听给某个一元素。实际应用的约束就是客户端内存的限制，这一点因每个浏览器而异
```javascript
var target = document.getElementById("test");
target.addEventListener('click',function test(){
        console.log("Hi");
},false)
target.addEventListener('click',function test(){
        console.log("Hello");
},false)
//Hi
//Hello
```
#### Inline events (“HTML onclick=“” property” and “element.onclick”)
使用方式：
* onclick=“”　
```html
<a id="test" href="#" onclick="function()">clickMe</a>
```
* element.onclick
```javascript
<a id="test" href="#">clickMe</a>
 
var target = document.getElementById('test')
target.onclick = function(){
    console.log('Hi');
}
```
* Inline events只能添加一个事件，如果同时有多个，只会输出最后一个的结果
```javascript
var target = document.getElementById('tttt')
target.onclick = function(){
    console.log('Hi');
}
target.onclick = function(){
    console.log('Hello');
}
//Hello
```

#### Inline events与Event Listeners的区别
Event Listeners可以添加无数个(理论上)事件，Inline events只能添加1个事件，且下面的会覆盖上面的。
