---
title: Array和String相互转换
date: 2018-10-29 19:56:53
tags: JavaScript
categories: 前端
---

Array 转 String、String 转 Array、字符串Array转换成数字Array
<escape><!-- more --></escape>

### 1 Array 转 String
#### String()
```JavaScript
String([1,2,3])
// "1,2,3"
String(['1', '2', '3'])
// "1,2,3"
```
#### toString()
```JavaScript
[1,2,3].toString()
// "1,2,3"
```
#### join()
```JavaScript
[1,2,3].join(",")
// "1,2,3"
[1,2,3].join("")
// "123"
```


### 2 String 转 Array
#### split()
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
#### Array from()
```JavaScript
Array.from('foo')
// ["f", "o", "o"]

// 将arguments 的值提出来。即，将类数组转为真数组。
function f() {
  return Array.from(arguments);
}
f(1, 2, 3);
// [1, 2, 3]
```
#### match()
```JavaScript
'hello'.match(/l/g)
// ["l", "l"]
```
#### slice()
```JavaScript
Array.prototype.slice.call('hello', 0,2)
//  ["h", "e"]
```


### 3 字符串Array转换成数字Array
如： `[var str = '1','2','3']=>[1,2,3]`
#### JSON.parse() + String()
```JavaScript
JSON.parse('['+ String(['1','2','3']) + ']')
// [1, 2, 3]
```
#### strArray.map(Number)
```JavaScript
['1','2','3'].map(Number)
// [1, 2, 3]
```

