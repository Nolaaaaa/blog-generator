---
title: 理解 Object.defineProperty 和 Proxy
date: 2019-03-14 23:13:36
tags: JavaScript
categories: 前端
---

理解 Object.defineProperty 和 Proxy
<escape><!-- more --></escape>

### 1 Object.defineProperty(obj, prop, descriptor)
`obj`：目标对象 
`prop`：需定义或修改的属性的名字
`descriptor`：目标属性所拥有的特性
#### 数据描述符有以下键值
```js
let obj = {}
Object.defineProperty(obj,"key",{
  value: '',
  writable: true | false,
  configurable: true | false,
  enumerable: true | false,
})
// value: 设置属性的值
// writable: 值是否可以重写。true | false
// enumerable: 目标属性是否可以被枚举。true | false
// configurable: 目标属性是否可以被删除或是否可以再次修改特性。 true | false
```
ps：一旦使用 `Object.defineProperty` 给对象添加属性，那么如果不设置属性的特性，那么 `configurable、enumerable、writable` 这些值都为默认的 `false` 。如果不使用 `Object.defineProperty` 添加属性，那么 `configurable、enumerable、writable` 这些值都为默认的 `true`

#### 存取描述符有以下键值
```js
let obj = {}
Object.defineProperty(obj,"key",{
  get:function (){} | undefined,
  set:function (value){} | undefined,
  configurable: true | false,
  enumerable: true | false,
})
// get: 给属性提供 getter 的方法，如果无 getter 则为 undefined
// set: 给属性提供 setter 的方法，如果无 setter 则为 undefined，设置的新值可通过value获取
```

ps：当使用了 `getter` 或 `setter`方法，不允许使用 `writable` 和 `value` 这两个属性。
使用示例：
```js
let obj = {}
let value = 'old value'
Object.defineProperty(obj,"key",{
  get:function (){
    //获取值时触发
    return value
  },
  set:function (newvalue){
    //设置新值时触发,设置的新值可通过 newvalue 获取
    value = newvalue
  }
})
//获取值
console.log( obj.key ) // old value

//设置值
obj.key = 'new value'
console.log( obj.key ) // new value
```

### 2 new Proxy(target, handler)
`Proxy` 对象用于定义基本操作的自定义行为（如属性查找，赋值，枚举，函数调用等），通过操作为对象生成的代理器，实现对对象各类操作的拦截式编程
`target`：要代理的目标对象（可以是任何类型的对象，包括原生数组，函数，甚至另一个代理）
`handler`：一个对象，其属性是当执行一个操作时定义代理的行为的函数。相当于拦截器，可以有多个拦截操作

使用示例：
```js
let handler = {
// set 有3个参数，obj（对象）, prop（属性）, value（新添加的值），get 有2个参数，obj, prop
set: function(obj, prop, value) {
  // 如参数名是 age 就执行如下的判断（报错则无法执行赋值操作，赋值无效）
  if (prop === 'age') {
    if (!Number.isInteger(value)) {
      throw new TypeError('The age is not an integer')
    }
    if (value > 200) {
      throw new TypeError('The age seems invalid')
    }
  }
  // 赋值
  obj[prop] = value;
  }
}

let person = new Proxy({}, handler)

person.age = 100
console.log(person.age) // 100

person.age = 'young' // 抛出异常: Uncaught TypeError: The age is not an integer
person.age = 300 // 抛出异常: Uncaught TypeError: The age seems invalid

person.name = 'Nola' // "Nola"
console.log(person) // Proxy {age: 100, name: "Nola"}
```

### 3 Object.defineProperty VS Proxy
Object.defineProperty缺点：
1. Object.defineProperty只能劫持对象的属性,因此我们需要对每个对象的每个属性进行遍历。
2. 在Vue中，Object.defineProperty无法监控到数组下标的变化，导致直接通过数组的下标给数组设置值，不能实时响应。

Proxy优点：
1. 可以劫持整个对象，并返回一个新对象。
2. Proxy可以直接监听数组的变化
3. 有多种劫持操作。

Proxy缺点：
1. Proxy是es6提供的新特性，兼容性不好，且这个属性无法用polyfill来兼容。

参考资料：
[ECMAScript 6 入门-Proxy](http://es6.ruanyifeng.com/#docs/proxy)
[defineProperty VS Proxy](http://www.10tiao.com/html/780/201812/2650588659/1.html)
[实现双向绑定 Proxy比 defineproperty优劣如何](https://juejin.im/post/5acd0c8a6fb9a028da7cdfaf)
