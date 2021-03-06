---
title: Array和String相互转换
date: 2018-10-29 19:56:53
tags: JavaScript
categories: 前端
---

Array 转 String、String 转 Array、字符串Array转换成数字Array
<escape><!-- more --></escape>

### Array 转 String
#### String()
```JavaScript
String([1,2,3])
// "1,2,3"
String(['1', '2', '3'])
// "1,2,3"
```
#### .toString()
```JavaScript
[1,2,3].toString()
// "1,2,3"
```
#### .join()
```JavaScript
[1,2,3].join(",")
// "1,2,3"
[1,2,3].join("")
// "123"
```


### String 转 Array
#### .split()
```JavaScript
// 一整个字符串
"123".split("")  
// ["1", "2", "3"]  

// 字符串中内容逗号分隔
"1,2,3".split(",")
// ["1", "2", "3"]  

"1,2,3".split("")
//  ["1", ",", "2", ",", "3"]

// 字符串中内容逗号分隔，逗号有空格
// 直接用split(",")，打印除了数字前会有空格,所以添加replace()去掉全部空格
"1, 2, 3".replace(/\s+/g,"").split(",")
//  ["1", "2", "3"] 
```
#### Array.from()
```JavaScript
Array.from('foo')
// ["f", "o", "o"]

// 将arguments 的值提出来。即，将类数组转为真数组。
function f() {
  return Array.from(arguments);
}
f(1, 2, 3)
// [1, 2, 3]
```
#### Array.prototype.slice.call()
```JavaScript
Array.prototype.slice.call('hello', 0, 2)
//  ["h", "e"]

function f() {
  return Array.prototype.slice.call(arguments);
}
f(1, 2, 3)
// [1, 2, 3]
```
#### .match()
```JavaScript
'hello'.match(/l/g)
// ["l", "l"]
```
#### 扩展运算符
扩展运算符可以将字符串转为真正的数组。
```JavaScript
[...'hello']
// [ "h", "e", "l", "l", "o" ]
```


### 字符串 Array 转换成数字 Array
如： `['1','2','3']=>[1,2,3]`
#### JSON.parse() + String()
```JavaScript
JSON.parse('['+ String(['1','2','3']) + ']')
// [1, 2, 3]
```
#### .map()
```JavaScript
['1','2','3'].map(Number)
// [1, 2, 3]
```


### Object 转 Array
```JavaScript
var obj = {one: 'hello', two: 'world'}
var arr = Object.keys(obj)
arr = arr.map(function(i){return obj[i]})
// ["hello", "world"]
```


### 伪数组 转 数组
#### Array.prototype.slice.call(arguments)
#### Array.from(arguments)
#### [...arguments]
```JavaScript
function foo() { 
  console.log(Array.prototype.slice.call(arguments))
  console.log(Array.from(arguments))
  console.log([...arguments]) 
}
foo(1,2,3,4)
```