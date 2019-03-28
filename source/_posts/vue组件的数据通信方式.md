---
title: vue组件的数据通信方式
date: 2019-03-28 21:44:11
tags: [Vue]
categories: 前端
---

三种通信方式整理
<escape><!-- more --></escape>

### 1  组件的数据通信方式—引用类型数据
`index.js`中传一个引用类型的数据`obj:{age: ..}`
```js
new Vue({
  data: {
    //传一个引用类型的数据
    obj: {
        age: 20 
    }
  }
}  
``` 
index.html 中加:obj="obj"  
```html
<body>
  <div id="app">
    <foot :obj="obj"></foot>
  </div>
</body>
```

Foot.vue 中加props: ['obj']接收
子组件件内部不能对父组件传的数据进行修改，所以，data中需加ob:obj，再在生命周期created()中设一个定时器修改值
```js
export default {
  props: ['obj'], 
  data() {
    return {
      ob: this.obj
    }
  },
  created(){
    setTimeout(() => {
      this.ob.age = 18
    },200)
  }
}
```
运行结果：
内部：ob.age=18
外部：obj.age=18  (obj的值被改变了 20-18)


### 2  组件的数据通信方式—自定义事件
如果把Foot.vue中的内部ob的值改成深拷贝的形式，则不会影响到外部obj的值
Foot.vue中
```js
data() {
  return {
    // ob: this.obj 改成：
    ob: window.JSON.parse(window.JSON.stringify(this.obj))
  }
},
```
运行结果：
内部ob.age=18   （深拷贝后，内部改变，外部不受影响）
外部obj.age=20

JSON 通常用于与服务端交换数据，在接收服务器数据时一般是字符串，可使用 `JSON.parse()` 方法将数据转换为 JavaScript 对象。
如果像 ob 改成深拷贝形式，同时也想让外面 obj 的值也改变，则需要用到自定义事件
Foot.vue中
```js
created(){
  setTimeout(() => {
    this.ob.age = 18
    this.$emit('change',18) // 把值传出去
  },4000)
}
```
在index.html中绑定自定义事件
```html
<foot :obj="obj" @change=“changeAge”></foot>
```

同时在index.js中定义方法接收
```js
changeAge(age){
  this.obj.age = age
  console.log(age)
}
```
运行结果：
内部ob.age=18
外部obj.age=18 (自定义事件使外部值改变)



### 3  组件的数据通信方式—全局事件
`var Event = new Vue()`;相当于又 new 了一个 vue 实例，Event 中含有 vue 的全部方法；
`Event.$emit(‘msg’,this.msg)`;发送数据，第一个参数是发送数据的名称，接收时还用这个名字接收，第二个参数是这个数据现在的位置；
`Event.$on(‘msg’,function(msg))`;接收数据，第一个参数是数据的名字，与发送时的名字对应，第二个参数是一个方法，要对数据的操作；
建立一个全局的foo.js，内容如下：
```js
import Vue from 'vue'
const foo = new Vue()
export default foo
```
在index.js中引入foo并在生命周期中添加监听
```js
import foo from 'js/foo.js'

//created中
created(){
  foo.$on('change',(age) => {
    this.obj.age = age
  })
}
```
Foot.vue中
```js
created(){
  setTimeout(() => {
    this.ob.age = 18
    //this.$emit('change',18)
    foo.$emit('change',18)  // 改成全局事件
  },4000)
}
``` 
不需要再用 changeAge 的方式监听
