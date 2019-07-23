---
title: JS原理实现
date: 2019-07-10 19:50:01
tags: JavaScript
categories: 前端
---

JS一些原理性的知识用函数自己实现
<escape><!-- more --></escape>


#### 实现一个 new 

```JS
const People = function(name) {
  this.name = name
}
People.prototype.sayName = function() {
  console.log('my name is ' + this.name)
}

// step1：创建一个新对象 obj
// step2：把 obj 的 __proto__ 指向 People.prototype 实现继承
// step3：执行构造函数，传递参数，改变this指向 People.call(obj, ...args)
// step4：结果是 null 和 undefined 时不处理
function _new(fn, ...arg) {
    let obj = Object.create(fn.prototype)
    let result = fn.call(obj, ...arg)
    return result instanceof Object ? result : obj
}

let my = _new(People, 'Nola')
```

#### 实现简单的双向绑定
查看效果：[地址](https://jsbin.com/sinuwufolo/2/edit?html,js,output)
```js
var input = document.getElementById('input')
var show = document.getElementById('show')
input.addEventListener('input', function(e) {
	obj.text = e.target.value
})

// defineProperty 实现
var obj = {}
Object.defineProperty(obj, 'text', {
  configurable: true,
  enumerable: true,
  get: function() {
    return obj.text
  },
  set: function(newValue) {
    input.value = newValue
    show.innerText = newValue
  }
})

// proxy 实现
var obj = new Proxy({}, {  
  get: function(obj, prop) {
    return obj[prop]
  },
  set: function(obj, prop, newValue) {
    obj[prop] = newValue
    input.value = newValue
    show.innerText = newValue
  }
})
```

