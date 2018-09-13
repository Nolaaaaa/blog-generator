---
title: 将数据保存到本地localStorage
date: 2018-08-05 17:14:22
tags: LocalStorage
categories: 前端
---

localStorage 类似于 [sessionStorage](https://developer.mozilla.org/en-US/docs/Web/API/Window.sessionStorage) 不同的是sessionStorage在页面被关闭时数据存储会被清除，而localStorage 是无期限的
<escape><!-- more --></escape>
### 1  什么是localStorage？
只读的localStorage 允许你访问一个 [Document](https://developer.mozilla.org/zh-CN/docs/Web/API/Document)  的远端（origin）对象  [Storage](https://developer.mozilla.org/zh-CN/docs/Web/API/Storage) 
```javascript
//增加数据
localStorage.setItem(``);
//读取数据
localStorage.getItem(``)
//移除数据
localStorage.removeItem(``)
// 移除所有
localStorage.clear();
```

### 2  将数据保存到本地localStorage中
```javascript
// 页面刷新后依然在本地保留数据
window.onbeforeunload = ()=>{       //页面刷新时调用
  let dataString = JSON.stringify(this.todoList)   //将todoList对象转化为JSON字符串 dataString
  window.localStorage.setItem('myTodos', dataString) //通过window.localStorage添加dataString数据到"myTodos"中
}
let oldDataString = window.localStorage.getItem('myTodos')  //通过window.localStorage读取"myTodos"中的数据
let oldData = JSON.parse(oldDataString)  //将读取到的JSON字符串解析JSON字符串
this.todoList = oldData || []    //把数据赋值给todoList对象
```
但是，`beforeunload` 事件里面的所有请求都发不出去，会被浏览器取消。
所以，`beforeunload`事件里不能将数据存到 LeanCloud的。

### 3  window.onbeforeunload 
当窗口即将被 [卸载（关闭）](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/onunload) 时会触发该事件，此时页面文档依然可见，且该事件的默认动作可以被 [取消](https://developer.mozilla.org/zh-CN/docs/Web/API/Event/preventDefault) 
`onunload`，`onbeforeunload`都是在刷新或关闭时调用，可以在<script>脚本中通过`window.onunload`来指定或者在<body>里指定。区别在于`onbeforeunload`在`onunload`之前执行，它还可 以阻止`onunload`的执行。
`onbeforeunload`是正要去服务器读 取新的页面时调用，此时还没开始读取；而`onunload`则已经从服务器上读到了需要加载的新的页面，在即将替换掉当前页面时调用。`onunload`是无 法阻止页面的更新和关闭的。而 `onbeforeunload`可以做到。

