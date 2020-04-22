---
title: JavaScript数据类型判断
date: 2018-04-28 16:22:07
tags: [JavaScript]
categories: 前端
---

Typeof()、instanceof运算符、Object.prototype.toString.call()、constructor属性都可以判断数据类型
<escape><!-- more --></escape>
### 1  Typeof()
判断基本数据类型（基本类型number、string、boolean、undefined，除了null。）⚠️不能区分对象、数组、null等
```javascript
//typeof()输出有五种数据类型  number   string    boolean   undefined   object   function

typeof("")  //"string"
typeof(1) //"number"
typeof(true) //"boolean"
typeof(undefined) //"undefined"
typeof({}) //"object"
typeof([]) //"object"                     array返回对象
typeof(function(){})  //"function"
typeof(null)  //"object"                  null返回对象

//但是怎么区分 对象、数组以及null 呢？
```

### 2  instanceof运算符
判断引用类型（引用类型，即对象类型。创建对象后可以调用这个对象下的方法有Object类型、Array类型、Date类型、RegExp类型、Function类型，包装类型（Boolean、Number、String）等。）
```javascript
//instanceof对引用类型进行判断
{} instanceof Object;     //true
[] instanceof Array;       //true
new Date() instanceof Date;        //true
function(){} instanceof Function;   //true

//instanceof无法对原始类型进行判断
"string" instanceof String;       //false
(111) instanceof Number;   //false
```

### 3  Object.prototype.toString.call()
能准确的判断基本类型和引用类型
```javascript
Object.prototype.toString.call('abc')     //"[object String]"
Object.prototype.toString.call(123)       //"[object Number]"
Object.prototype.toString.call(true)      //"[object Boolean]"
Object.prototype.toString.call(undefined) //"[object Undefined]"
Object.prototype.toString.call(null)      //"[object Null]"

Object.prototype.toString.call({})        //"[object Object]"
Object.prototype.toString.call([])        //"[object Array]"
Object.prototype.toString.call(function(){}) //"[object Function]"
```

### 4  constructor属性
Constructor属性始终指向创建当前对象的构造函数
```javascript
"string".constructor == String   //true
true.constructor == Boolean   //true
(123).constructor == Number   //true
{}.constructor == Object   //true
[].constructor == Array   //true
```
一个常用的函数
```javascript
function isArray(arr){
    return typeof arr == “object” && arr.constructor == Array;
}
```