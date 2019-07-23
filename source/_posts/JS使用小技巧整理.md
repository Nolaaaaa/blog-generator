---
title: JS使用小技巧整理
date: 2019-07-15 19:49:21
tags: JavaScript
categories: 前端
---

一些JS使用的小技巧整理，如快速生成随机数，使对象可迭代、将多维数组展平等
<escape><!-- more --></escape>


### 将多维数组展平
**第一种：不指定展平级别，只展平一级**
```js
var arr = ['a', 'b', ['c', ['d']]];

var flat1_1 = [].concat.apply([], arr);
var flat1_2 = [].concat(...arr);
var flat1_3 = arr.flat();

console.log(flat1_1,flat1_2,flat1_3)   // ["a", "b", "c", ["d"]]
```
**第二种：不指定展平级别，能把数组完全展开**
```js
// 把数组转化成字符串再转化成数组（不能指定递归深度）
var flat2_1 = arr => arr.toString().split(',')  // toString() 也可使用join(',')替代

// 递归➕map（不能指定递归深度）
var flat2_2 = arr => [].concat(...arr.map(i=>Array.isArray(i)?flat2_2(i):i))

// 利用 flat() 函数
var flat2_3 = arr.flat(Infinity)

console.log(flat2_1(arr), flat2_2(arr), flat2_3)   // ["a", "b", "c", "d"]
```
**第三种：能指定展平级别**
```js
// 利用 flat() 函数
var flat3_1 = arr.flat(2)  // 展平两级

// 普通递归（能指定递归深度）
var flat3_2 = (arr, num) => {
  let result = []
  if(!num) num = 1
  function temp(arr) {
    arr.forEach((i,index) => {
      if(Array.isArray(i)) {
        if(num != 1) {
          num--
          temp(i)
        } else {
          result.push(...i)
        }
      } else {
        result.push(i)
      }
    })
  }
  temp(arr)
  return result
}

console.log(flat3_1,flat3_2(arr, 5))   // ["a", "b", "c", "d"]
```

### 字符串大小写取反
```js
var result = str => {
  let newStr = ''
  for(let i of str) {
    if(/[a-z]/.test(i)) {
      i = i.toUpperCase() 
    } else if(/[A-Z]/.test(i)) {
      i = i.toLowerCase() 
    }
    newStr += i
  }
  return newStr
}
console.log(result('aBc'))  // "AbC"
```

### 快速过滤数组false值
```js
let arr = [0,1,2,3,4,true,false,undefined,null,'']
arr.filter(Boolean)
```

### 生成随机数
```JS
// 可设置随机数长度
let randomId = len => Math.random().toString(36).substr(3, len)
console.log(randomId(10))   // sj2ymh25xj

// 生成随机颜色
let randomColor = () => "#" + Math.floor(Math.random() * 0xffffff).toString(16).padEnd(6, "0")
console.log(randomColor())  // #8980eb

// 生成范围随机数
let randomNum = (min, max) => Math.floor(Math.random() * (max - min + 1)) + min;
console.log(randomNum(1, 10))

// 数组值随机排列
let arr = [0, 1, 2, 3, 4].slice().sort(() => Math.random() - .5) 
console.log(arr)    // [2, 3, 0, 1, 4]

// 获取随机数组成员
let randomItem = arr[Math.floor(Math.random() * arr.length)]
```

### 生成星级评分
```js
let startScore = rate => "★★★★★☆☆☆☆☆".slice(5 - rate, 10 - rate)
console.log(startScore(5))  // "★★★★★"
```

### 获取URL查询参数
```js
// location.search = "?name=Nola&age=24"
let params = new URLSearchParams(location.search.replace(/\?/ig, ""))  
params.has("Nola") // true
params.get("age")  // "24"
```

### 快速生成数组
```JS
// 生成连续数组
[...Array(5).keys()]   // [0, 1, 2, 3, 4]

// 生成值一样的数组
Array(5).fill(1)   // [1, 1, 1, 1, 1]
```

### 一个数字变成千分位
```js
let numFormat = num => (num.toString().indexOf ('.') !== -1) ? num.toLocaleString() : num.toString().replace(/(\d)(?=(?:\d{3})+$)/g, '$1,')
```

### 用代码画一个键盘
```js
(_=>[..."`1234567890-=~~QWERTYUIOP[]\\~ASDFGHJKL;'~~ZXCVBNM,./~"].map（x=>（o+=`/${b='_'.repeat（w=x<y?2:' 667699'[x=["BS","TAB","CAPS","ENTER"][p++]||'SHIFT',p]）}\\|`,m+=y+（x+'    '）.slice（0,w）+y+y,n+=y+b+y+y,l+=' __'+b）[73]&&（k.push（l,m,n,o）,l='',m=n=o=y）,m=n=o=y='|',p=l=k=[]）&&k.join`
`)()
```


### 取整 ~~ 和 | 0 和 >> 0
对正数来说 `~~` + `| 0`  `>> 0` 运算结果与 Math.floor( ) 运算结果相同，而对于负数来说与Math.ceil( )的运算结果相同
```js
~~2.5 === Math.floor(2.5)   // 2
~~-2.5 === Math.ceil(-2.5)  // -2

2.5 | 0        // 2
-2.5 | 0       // -2

2.5 >> 0        // 2
-2.5 >> 0      // -2
```

### 判断是否是奇数 & 1 和 % 2
```js
// 判断是否是奇数
var num = 4
!!(num & 1)   // false
!!(num % 2)   // false

let oddEven = num => !!(num & 1) ? "odd" : "even"
console.log(oddEven(2))
```

### 利用短路运算符 || 和 && 替代简单的if语句
`||`返回遇到的第一个真值，`&&`返回遇到的第一个假值
```js
(A || B) && fun()  // 相当于 if(A || B) fun()
A && B && fun()    // 相当于 if(A && B) fun()
```

### 数组中取最大/小值
```JS
let min = Math.min(...arr)
let max = Math.max(...arr)
```

### 给git配置别名
```js
// 给 commit 起一个别名 ci
$ git config --global alias.ci commit
```

### 创建一个可迭代对象
```js
var foo = {
	0 : '00',
	1 : '11',
	2 : '22',
	3 : '33',
	length : 4
}
// 使对象可迭代 如果没有这一步，会报 object is not iterable 的错
foo[Symbol.iterator] = function() {
	let i = 0, that = this
	return {
		next() {
			return i < that.length ? { done: false, value: that[i++] } : { done: true }
		}
	}
}
// Set函数接受一个具有 iterable 接口数据结构，否则会报错
new Set(foo)  // Set(4) {"00", "11", "22", "33"} 
```
