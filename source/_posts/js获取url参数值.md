
---
title: js获取url参数值
date: 2018-12-01 19:07:58
tags: JavaScript
categories: 前端
---

js获取url参数值的各种方法
<escape><!-- more --></escape>

### 用正则的方法匹配
```js
function GetValue() { 
  // 把地址？后面的值匹配出来 如：?token=1&from=2
  var url = location.href.match(/\?.*/)[0]
  var map = new Object(); 
  if (url.indexOf("?") != -1) {
    // 删掉问号 如：token=1&from=2
    var str = url.substr(1); 
    // 多个参数变成一个数组 如：{"token=1","from=2"]
    var strs = str.split("&"); 
    for(var i = 0; i < strs.length; i ++) {
      // 把参数的值提取出来 如：["token","1"],["from","2"]
      var temp = strs[i].split("=")
      // 第一个值为key，第二个值为value 如：{"token": "1"}
      map[temp[0]]=unescape(temp[1])
    } 
  } 
  return map; 
} 
var value = GetValue()
// 查找参数from的值
console.log(value.from)
```