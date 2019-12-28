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
var arr = ['a', 'b', ['c', ['d']]]

var flat1_1 = [].concat.apply([], arr)
var flat1_2 = [].concat(...arr)
var flat1_3 = arr.flat()

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
let randomNum = (min, max) => Math.floor(Math.random() * (max - min + 1)) + min
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

### 使 a === a - 1 为true
使用 Infinity 可以做到，但是怎么才能得到正负Infinity的值？方法一：数值运算的值，超过了Number允许表示的范围；方法二：将一个不为0的正负数除以0
```JS
// 无穷小的数
a = -Infinity
console.log(a === a - 1)  // true

// 无穷大的数
a = Infinity
console.log(a === a - 1)  // true

// 无穷数的运算
Infinity + Infinity  // Infinity
Infinity - Infinity  // NaN
Infinity * Infinity  // Infinity
Infinity / Infinity  // NaN
Infinity * 0         // NaN
```

### 使 a == 1 && a == 2 && a == 3 返回 true
在引擎读取 a 的值时，在方法内部做处理
**实现宽松相等 ==**
```JS
// 方法一：使用 toString 或者 valueOf
a = {
  i: 1,
  toString: () => a.i++, 
  valueOf: () => a.i++,
}

// 方法二：使用 Proxy 
a = new Proxy({ i: 1 }, {
  get(obj) { return () => obj.i++ }
}) 

console.log(a == 1 && a == 2 && a == 3)  // true
```
**实现严格相等 ===**
```js
// 方法一：
i = 1
Object.defineProperty(window, 'a', {
  get: () => i++ 
})

// 方法二：
value = function* () {
  let i = 1
  while(true) yield i++
}()

Object.defineProperty(window, 'a', {
  get() {
    return value.next().value
  }
})

console.log(a === 1 && a === 2 && a === 3)  // true
```


### 使 a == b && a == c && c != b 为 true
[代码实现](https://jsbin.com/qabiwadeli/1/edit?js,console)
```JS
let a,b,c
// 方法一：利用复杂类型指向引用的特性，将 b,c 设置为复杂类型
// "[]" != []
a = false, b = [], c = []
console.log( '结果--array', a == b && a == c && c != b )   // true

// "{}" != {}
a = "[object Object]", b = {}, c = {}
console.log( '结果--object', a == b && a == c && c != b )   // true

// "[object Function]" != function(){}
a = "function(){}", b = function(){}, c = function(){}
console.log( '结果--function', a == b && a == c && c != b )   // true

// 方法二：利用proxy劫持
b = 1, c = 2
a = new Proxy({ i: 1 }, { 
  get(obj) { return () => obj.i++ } 
})
console.log( '结果--proxy', a == b && a == c && c != b )   // true
```


### 为什么["1", "2", "3"].map(parseInt) 返回值是[1, NaN, NaN]
[map()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/map)调用callback函数时，会给它传递三个参数：当前**正在遍历的元素**、**元素索引**、**原数组本身**
[parseInt](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/parseInt)接受两个参数：**元素**、**进制数**
遍历时parseInt的第二个参数被传进来的是元素的索引值，所以...
```JS
["1", "2", "3"].map(parseInt) 
// [1, NaN, NaN]

["1", "2", "3"].map(item => parseInt(item))
// [1, 2, 3]
```

### 利用 a 标签解析 URL
```JS
function parseURL(url) {
  let a =  document.createElement('a')
  a.href = url
  return {
    host: a.hostname,
    port: a.port,
    query: a.search,
    params: (function(){
      var result = {}, str = a.search.replace(/^\?/,'').split('&')    
      for (let i in str) {
        let s = str[i].split('=')
        result[s[0]] = s[1]
      }
      return result
    })(),
    hash: a.hash.replace('#','')
  }
}
parseURL('https://www.google.com:8008?a=1&b=2')

// 其他快速url参数的方法
new URLSearchParams(location.search).get("a")  // 1
```


### 删除链接中某个参数
[URLSearchParams MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/URLSearchParams)
```JS
// 利用URLSearchParams
function delSearchParam(name, search = location.search) {
  let _params = new URLSearchParams(search.substr(1))
  _params.delete(name)
  return `?${_params.toString()} `
}

// 普通的
function delSearchParam(name, search = location.search) {
	let _arr = search.split("?")[1].split("&")
	// 	参数变成数组
	let _map = {}
	for(let i = 0; i < _arr.length; i++) {
		let _t = _arr[i].split("=")
		_map[_t[0]] = _t[1]
  }
	// 	删除这个参数
	if(_map[name]) {
		delete _map[name]
  }
	// 	把删除后的数组变成字符串
	let _result = '?'
	for(let i in _map) {
		_result += `${i}=${_map[i]}&`
  }
	// 	删除字符串最后一个元素
	return _result.substr(0, _result.length-1)
}

delSearchParam('a', '?a=1&b=2&c=3')
```


### 实现吸顶
[参考文章--交叉观察者](https://juejin.im/post/5d665133e51d4561c83e7c83)
[MDN--IntersectionObserver](https://developer.mozilla.org/zh-CN/docs/Web/API/IntersectionObserver)
**方法一**
[效果](https://jsbin.com/qojufiputu/edit?html,css,output)
```CSS
.target {
  position: sticky;
  top: 0;
}
```
使用条件：
1. 父元素不能`overflow:hidden`或者`overflow:auto`属性
2. 必须指定`top、bottom、left、right`4 个值之一，否则只会处于相对定位
3. 父元素的高度不能低于`sticky`元素的高度
4. `sticky`元素仅在其父元素内生效

**方法二**
[效果](https://jsbin.com/hekoketolu/1/edit?html,css,js,output)
```CSS
var a = document.querySelector('.a')
var b = document.querySelector('.b')
new IntersectionObserver(function(e) {
  let offsetTop = a.getBoundingClientRect().top
  if(offsetTop < 0) {
    a.style.position = 'fixed'
    a.style.top = 0
    a.style.left = '50%'
    a.style.transform = 'translateX(-50%)'
  } else {
    a.style.position = 'relative'
  }
}, {
  threshold: [1]
}).observe(a)
```

### 时间相关的计算
```JS
// 计算当前日期天数
let dayOfYear = date => 
  Math.floor((date - new Date(date.getFullYear(), 0, 0)) / (1000 * 3600 * 24))

dayOfYear(new Date()) // 301

// 时间戳转日期时间
let getDate = time => 
  new Date(time).toLocaleDateString().replace(/\//g, "-") + " " + new Date(time).toTimeString().substr(0, 8)

getDate(Date.now())  // "2019-10-28 17:36:47"
```

### 实现深拷贝
[代码实现](https://jsbin.com/gogopeyana/1/edit?js,console) [参考文章](https://juejin.im/post/5d6aa4f96fb9a06b112ad5b1) [Map MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Map)
PS：扩展符`...`可实现数组的深拷贝


### 防抖
防抖查看效果：[地址](https://jsbin.com/religexeka/61/edit?html,js,output)
```JS
// 触发间隔超过指定间隔的任务才会执行，连续发生动作会刷新时间
function debounce(fn, delay, ...args) {
  let timer = null
  return function () {
    clearTimeout(timer)
    timer = setTimeout(() => {
      fn.apply(this, args)
    }, delay)
  }
}
```

### 节流
节流查看效果：[地址](https://jsbin.com/religexeka/61/edit?html,js,output)
```JS
// 在指定间隔内任务只执行一次，连续发生的动作会被忽略
function throttle(fn, delay, ...args) {
  let prev = Date.now()
  return function () {
    let now = Date.now()
    if (now - prev >= delay) {
      fn.apply(this, args)
      prev = Date.now()
    }
  }
}

function throttle(fn, delay, ...args) {
  let timer
  return function () {
    if (!timer) {
      timer = null
      timer = setTimeout(() => {
        timer = null
        fn.apply(this, args)
      }, delay)
    }
  }
}
```