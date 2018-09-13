---
title: 全局变量变局部变量和MVC思想
date: 2018-06-15 20:18:36
tags: MVC
categories: 前端
---

主要介绍了函数中的全局变量如何变成局部变量以及MVC思想
<escape><!-- more --></escape>
### 1 函数中的全局变量如何变成局部变量？
1. 全局变量之间会相互骚扰。所以在代码中不要用全局变量。ES6之前只有函数里面有全局变量。

2. 全局变成局部变量怎么变？
* 把代—放在一个函数如中，再.call()执行一下这个函数？行不行？
* 不行—样的话函数名也是一个全局变量（全局函数）。
* 那么—掉函数名把函数变成一个匿名函数？再function(){}.call()立即执行，这样 可以，但是Chrome报错，语法错误。

3. 全局变量变局部变量的方法：
* 方法一：!function(){}.call( )    
（前面加+、-、!都可以，这种方法会改变函数的返回值，但是不在乎这个函数的返回值的话加个取反没有关系）	
* 方法二：（function(){}).call( )
（用括号把函数括起来。但是不推荐这种做法，因为如果（函数）的前一行被加上一个xxx，很容易被浏览器误解为是在xxx()。）

### 2 MVC思想

1. 什么是MVC思想
MVC 是一种设计模式（或者软件架构），把系统分为三层：Model数据、View视图和Controller控制器。
Model 数据管理，包括数据逻辑、数据请求、数据存储等功能。前端 Model 主要负责 AJAX 请求或者 LocalStorage 存储
View 负责用户界面，前端 View 主要负责 HTML 渲染。
Controller 负责处理 View 的事件，并更新 Model；也负责监听 Model 的变化，并更新 View，Controller 控制其他的所有流程。
Model和服务器交互，Model 将得到的数据交给 Controller，Controller 把数据填入 View，并监听 View。用户操作 View，如点击按钮，Controller 就会接受到点击事件，Controller 这时会去调用 Model，Model 会与服务器交互，得到数据后返回给 Controller，Controller 得到数据就去更新 View。
![avatar](https://i.loli.net/2018/06/15/5b23b83c39a41.png)

2. MVC思想的由来
MVC是XeroxPARC在八十年代为编程语言Smalltalk发明的一种软件设计模式，至今已被广泛使用。

4. VC 第一版
```javascript
!function(){ 
    var view = document.querySelector(‘xxx')
    var controller = function(view){
        ..…   }
    controller.call(null,view)
}.call()     
```

5. VC 第二版，添加`init`
```javascript
!function(){
  var view = document.querySelector('xxx')
 	var controller = {
    view: null,
    init: function(view){
        this.bindEvents().    //this.bindEvents().call(this)
    },
    bindEvents: function(){
        ...
… }
}
  controller.init(view).      //controller.init.call(controller,view)
}.call()         
```

6. MVC思想完整版
```javascript
!function(){
	  //M 模型--用于数据储存，负责和server数据交互
    var model = {
      fetch: function(){	
			...
      },
      save: function(){  
			...
      }
    }

	  //V 视图--负责视图展示
    var view = document.querySelector('xxx')

	  //C 控制--负责业务逻辑
    var controller = {
      view: null,
      model: null,
      init: function(view,model){
        this.view = view
        this.model = model
        this.bindEvents()
      },
      bindEvents: function(){     
			...
      }
  }
  controller.init(view,model)
}.call()
```
