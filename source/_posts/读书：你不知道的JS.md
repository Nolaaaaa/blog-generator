---
title: 读书：你不知道的JS
date: 2019-08-31 20:44:10
tags: [JavaScript]
categories: 前端
---

【你不知道的JS】一共上中下三册，包含内容有：作用域和闭包、this和对象原型、类型和语法、异步和性能、ES6及更新版本等内容，整理本篇博客目的主要是整合所学基础知识
<escape><!-- more --></escape>

### 作用域和闭包
#### 引擎、编译器、作用域
**代码执行前的编译会经历的三个步骤：**
1. 词法分析：将字符组成的**字符串**分解成有意义的**代码块（词法单元）**，会有特定步骤进行性能优化
2. 语法分析：将**词法单元**转化成一个**抽象语法树（AST）**，会有特定步骤进行性能优化
3. 生成代码：将**AST**转化为**可执行代码**

**引擎、编译器、作用域的任务：**
1. 引擎：负责JS的编译、执行过程
2. 编译器：负责语法分析、代码生成
3. 作用域：收集维护变量、有一套严格的规则（如：词法作用域）

**当你给一个变量（函数）赋值时：**
1. 声明：**编译器**询问**作用域**是否存在该变量，如没有，则进行声明
2. 查找：运行时**引擎**在**作用域**查找该变量，找到就给它赋值

**引擎查找类型分为 LHS 和 RHS：**
1. LHS：找到**变量容器**并进行**赋值**（非严格模式下找不到变量时会自动声明，否则抛出`ReferenceError`）
2. RHS：找到变量的**源值**（找不到时会抛出`ReferenceError`、查找到变量但是进行不合理的操作时会抛出`TypeError`）
```js
function foo(a) {
  console.log(a)   // RHS
}
foo(2)  // foo 使用了 RHS，2 是对变量 a 进行赋值所以使用的是 LHS
```

**作用域嵌套：**
1. 引擎无法在当前作用域查找到变量时，会向上一级继续查找，直到最外层的全局作用域

#### 作用域规则--词法作用域
**作用域的两种工作模型：**
1. 动态作用域：**运行**时确定，关注在何处**调用**（和JS中的this很像）
2. 词法作用域：**定义**时确定，关注在何处**声明**，只会查找一级标识符，如：`a.b.c`，只会查找`a`（JS所使用的）

**欺骗词法作用域的方法：**
1. `eval(...)`：非严格模式下：eval 所执行的代码如果**包含变量（函数）声明**，在运行时能**对词法作用域进行修改**，严格模式下 eval 有自己的作用域
2. `with(...)`：非严格模式下：通常被当作重复引用同一个对象中多个属性的快捷方式，实际是根据传递的对象**创建新的词法作用域**，严格模式下会被禁止
```js
function foo(str, a) {
  eval(str)          // 在执行时声明了 b
  console.log(a, b)  // 2 3
}
foo("var b = 3", 2)
```

**欺骗词法作用域导致的性能问题：**
1. 问题：引擎无法在编译时对作用域查找进行优化，导致代码运行变慢
2. 原因：有些依赖于对词法进行静态分析，预先确定变量函数定义的位置，以便快速找到标识符，但是如果发现`eval`和`with`，引擎只能假设标识符的位置判断都是无效的

#### 函数作用域和块作用域
**什么是函数作用域和块作用域：**
1. 函数作用域：有作用域气泡，这个函数的全部变量都可以在函数范围内使用及复用
2. 块作用域：ES6 中的`const`和`let`可声明块级作用域，在块`{}`范围内使用和复用，除此之外还有`try/catch`，`with`

**函数作用域可以用来做什么？**
1. 隐藏代码：把变量和函数包裹在一个函数作用域中，用这个作用域隐藏它们

**隐藏代码的好处：**
1. 规避冲突：避免同名标识符之间的冲突（规避冲突的两种解决方式：全局命名空间、模块管理工具）

**函数名本身会污染所在全局作用域，怎么解决？**
1. 使用立即执行函数表达式`(function foo() {..})()`，将变量名隐藏在自身中（此时`foo`只能在`...`处被访问），也称`IIFE`（也可以是匿名）

**区分函数声明和函数表达式：**
1. 函数声明：`function`是声明的第一个词，名称标识符绑定在所在作用域
2. 函数表达式：`function`不是声明的第一个词，名称标识符绑定在函数自身


#### 变量提升
1. 变量提升：即所有声明（变量和函数）被移动到各自作用域的最顶端（`const`和`let`不存在变量提升）
2. 提升顺序：函数会先提升，然后是变量
3. 存在同名声明时：如一个函数声明一个 var 声明，var 声明会被忽略掉，但是后续的函数声明能覆盖前面的

#### 闭包
1. 什么是闭包：当一个函数记住并访问所在词法作用域时就产生了闭包，即使函数是在当前词法作用域之外执行
2. 闭包的作用：保持对一个作用域的引用，使作用域一直存活
3. 回调函数与闭包：**定时器**、**事件监听器**、**Ajax请求**、**跨窗口通信**、**web workers**或其他异步（或同步任务）中，只要使用了回调函数，实际上就是在使用闭包
4. 模块与闭包：调用模块中的方法时，实际就是在使用闭包，没有闭包的模块不是真正的模块
5. 循环与闭包：循环过程中每个迭代都需要一个闭包作用域
```js
for(var i = 1; i < 4; i++) {
  setTimeout(() => {
    console.log(i)   // 4 4 4
  }, i*1000)
}

// 利用 IIFE 创建一个作用域，并创建一个变量用来保存每个迭代中的 i
for(var i = 1; i < 4; i++) {
  (function() {
    var j = i
    setTimeout(() => {
      console.log(j) // 1 2 3
    }, j*1000)
  })()
}

for(var i = 1; i < 4; i++) {
  (function(j) {
    setTimeout(() => {
      console.log(j) // 1 2 3
    }, j*1000)
  })(i)
}

// 给 setTimeout 传递第三个参数，定时器到期就传递给定时器中的函数
for(var i = 1; i < 4; i++) {
  setTimeout((j) => {
    console.log(j)  // 1 2 3
  }, i*1000, i)
}

// 使用 let 劫持作用域
for(var i = 1; i < 4; i++) {
  let j = i
  setTimeout(() => {
    console.log(j)  // 1 2 3
  }, j*1000)
}

for(let i = 1; i < 4; i++) {
  setTimeout(() => {
    console.log(i)  // 1 2 3
  }, i*1000)
}
```

### this和对象原型
#### 关于this
1. `this`是在运行中被调用时绑定的，它不指向函数本身，也不指向函数的词法作用域
2. 函数如何引用自身？第一种：具名函数可在函数内部通过函数名引用自身，第二种：匿名函数可通过`arguments.callee`引用自身（被弃用了），第三种：可通过call将this绑定在自身上
3. 匿名函数的缺点？第一：调试栈更难追踪，第二：自我引用（递归、事件绑定解除等）更难，第三：代码（稍微）更难理解

#### this的绑定规则
1. **默认绑定：**非严格模式下绑定到全局对象，严格模式下绑定大到`undefined`（函数体是否严格，而非调用位置）
2. **隐式绑定：**如果调用位置有上下文对象，会绑定到这个上下文对象上。注意⚠️：隐式绑定后不能再赋值给另一个变量进行调用，否则会造成**隐式丢失**，如：`a.b`中b的this指向a，`c = a.b`中b的this就会指向全局而不是a，解决办法：使用硬绑定`c = b.bind(a)`可使b中的this指向a
3. **显式绑定：**使用`bind`、`call`、`apply`硬绑定指定this，但是如果传入`null``undefined`会被忽略，会进行默认绑定，所以如果显式绑定一个空对象，可以使用`Object.create(null)`创建一个没有原型的空对象传进去
4. **new绑定：**构造函数（使用new时被调用的函数）中的this指向它的实例对象
5. 如何确定this应用于哪条绑定规则？先通过调用位置判断，如果一个位置可以应用多条规则，则通过优先级确定
6. 优先级：默认绑定 < 隐式绑定 < 显式绑定 < new绑定
7. 注意⚠️：调用间接引用的函数会应用默认绑定的规则
8. 固定this：回调函数造成的this丢失可以通过固定this来解决，第一种：词法作用域风格--箭头函数，第二种：词法作用域风格--将this赋值给一个变量，第三种：this风格的绑定bind(this)，存在词法作用域风格的代码和this风格，在代码最好只保持一种风格

**使用new时会发生什么？**
1. 创建一个新对象（空对象）
2. 新对象的原型指向构造函数的原型
3. 新对象绑定到构造函数中的this
4. 返回新对象

#### 关于对象
1. 定义方式：声明形式、构造形式
3. 属性名：永远是字符串，且可通过`[变量 + 变量]`形式计算出来
4. 复制：浅拷贝（`Object.assign({}, obj)`） + 深拷贝

**对象属性：**
1. 属性描述符：设置属性的特性，`writable: false`使属性**不可修改**，`configurable: false`使属性**不可重定义、不可删除**，`enumerable: false`使属性**不可枚举**。相关博客：[Object.defineProperty和Proxy](https://nolaaaaa.github.io/2019/03/14/%E7%90%86%E8%A7%A3-Object-defineProperty-%E5%92%8C-Proxy/)
2. 查看属性描述符：`Object.getOwnPropertyDescriptor(obj, 'key')`
7. 禁止对象扩展：`Object.preventExtensions(obj)`使对象**不可添加新属性**，但是现有属性可修改、可删除。判断是否可扩展：`Object.isExtensible(obj)`
8. 密封对象：`Object.seal(obj)`使对象不可添加新属性，现有属性**不可重定义、不可删除**，但是可修改（相当于`Object.preventExtensions(obj)` + `configurable: false`）。判断是否密封：`Object.isSealed(obj)`
9. 冻结对象：`Object.freeze(obj)`使对象不可添加新属性，现有属性不可重定义、不可删除、**不可修改**（相当于`Object.seal(obj)` + `writable: false`）。判断是否冻结：`Object.isFrozen(obj)`
10. 获取对象的属性：`Object.keys()`返回对象**自身**的**可枚举**的属性

**遍历对象：**
1. `for in`：遍历的key，检查**自身 + 原型链**中的属性，遍历**可枚举**的属性
2. `for of`：遍历的是value，向对象请求一个迭代器，使用迭代器的`next()`方法进行遍历，对象没有内置迭代器，所以需要自定义对象迭代器`@@iterater`（原型中存在`Symbol.iterator`属性），相关博客：[JS使用小技巧整理--创建一个可迭代对象](https://nolaaaaa.github.io/2019/07/15/JS%E4%BD%BF%E7%94%A8%E5%B0%8F%E6%8A%80%E5%B7%A7%E6%95%B4%E7%90%86/#more)

**对象存取值：**
11. 取值：触发[Get]。查找属性步骤：存在存取操作符`getter`？获取`getter`的返回值 => 在对象中找 => 在原型中找 => 返回`undefined`
12. 赋值：触发[Put]。检查操作属性：存在存取操作符`setter`？调用`setter`进行赋值 => `writable`为`false`？赋值无效 => 都不是？赋值成功

**检查属性是否存在：**
1. `hasOwnProperty()`：只会检查**自身**，不会检查原型，所以不能判断没有原型的对象如`Object.create(null)`用法：`Object.prototype.hasOwnProperty.call(obj,'属性名')`  [示例](https://jsbin.com/gevudomilu/1/edit?js,console)
2. 使用`in`操作符：检查**自身 + 原型链**中的属性，**包括不可枚举**的（如果是数组：只会检查下标）

#### 关于类
1. 类是一种设计模式，使用new可以将类实例化
2. 面向类的设计模式：实例化、继承、多态
3. 混入模式（mixin）：可以用来模拟类的复制行为，但是有很多隐患

#### 原型
**[[prototype]]**
1. 对象之间通过内部的`[[prototype]]`关联
2. 所有`[[prototype]]`的终点都是`Object.prototype`
3. 修改`prototype`后，新`prototype`的`constructor`属性（不可枚举、可修改）不会自动获得，需要手动赋值

**Object.create()**
1. 创建一个对象，并把这个对象的`[[prototype]]`关联到指定的对象
2. `Object.create(null)`会创建一个没有原型链的对象，不会受原型链干扰，适合存储数据
3. `Object.create()`的`polyfill`
```js
Object.create = function(obj) {
	function F() {}
	F.protoptype = 0
	return new F()
}
```

**Object.getPrototypeOf()、Object.setPrototypeOf()、isPrototypeOf()**
```js
// Object.getPrototypeOf() 查看某个对象的原型
function A() {}
let a = new A()
Object.getPrototypeOf(a) === A.prototype 
a.__proto__ === A.prototype  // __proto__ 非标准

// isPrototypeOf() 测试一个对象是否存在于另一个对象的原型链上
A.prototype.isPrototypeOf(a)

// Object.setPrototypeOf() 关联两个对象的原型
a.prototype = Object.create(b.prototype) // ES5 抛弃默认的a.prototype（需要进行垃圾回收）
Object.setPrototypeOf(a.prototype, b.prototype) // ES6 直接修改现有的a.prototype

```

#### 行为委托
1. 内部委托比起直接委托可以让API接口设计更加清晰
2. 行为委托认为对象之间的兄弟关系，相互委托，而非父子关系，[[prototype]]机制本质上就是行为委托
3. 两种设计模式：**面向对象风格**（类风格，构造函数、原型、new）、**对象关联风格**（委托风格，直接穿件和关联对象，使用基于[[prototype]]的行为委托，干净简洁）
4. ES6的Class是类风格的语法糖

### 类型和语法
我的另外几篇关于数据类型的博客：[数据类型介绍](https://nolaaaaa.github.io/2018/04/26/JavaScript%E6%95%B0%E6%8D%AE%E7%B1%BB%E5%9E%8B%E4%BB%8B%E7%BB%8D/) [数据类型判断](https://nolaaaaa.github.io/2018/04/28/JavaScript%E6%95%B0%E6%8D%AE%E7%B1%BB%E5%9E%8B%E5%88%A4%E6%96%AD/) [数据类型转换](https://nolaaaaa.github.io/2018/04/27/JavaScript%E6%95%B0%E6%8D%AE%E7%B1%BB%E5%9E%8B%E8%BD%AC%E6%8D%A2/)


### 异步和性能


### ES6及更新版本等内容
具体可参考阮一峰大大的 [ECMAScript入门](http://es6.ruanyifeng.com/)