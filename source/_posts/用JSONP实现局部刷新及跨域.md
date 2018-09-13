---
title: 用JSONP实现局部刷新及跨域
date: 2018-06-1 22:43:27
tags: JSONP
categories: 前端
---

实现HTML页面局部刷新有哪些方法？什么是JSONP？JSONP怎么实现局部刷新和跨域？
<escape><!-- more --></escape>
### 1  实现HTML页面局部刷新的方法
也可以用iframe方法发get请求，但是目前iframe基本已经被弃用，所以此处就不介绍这个方法。
方案一：用图片造 get 请求
方案二：用 script 造 get 请求(用script发请求有个问题，不管成功或者失败，都会生成一个script并执行其中的内容。)

### 2  什么是JSONP？
请求方创建 script，src 指向响应方，同时传一个查询参数 ?callbackName=yyy 
响应方根据查询参数callbackName，
构造形如 yyy.call(undefined, ‘你要的数据’) yyy(‘你要的数据’) 这样的响应 
浏览器接收到响应，就会执行 yyy.call(undefined, ‘你要的数据’) 
请求方就知道了他要的数据 
这就是 JSONP 。简单说就是script加callback参数。
JSONP的主要方法是通过动态创建script标签并配置的src属性，然后加入页面，触发浏览器加载并执行相应的 JavaScripts 代码，以实现无刷新数据交互的效果。
约定：
callbackName -> callback 
yyy -> 随机数 frank12312312312321325()

### 3  用JSONP实现局部刷新
```javascript
button.addEventListener('click', (e)=>{
    let script = document.createElement('script')
    let functionName = 'dsfnd'+ parseInt(Math.random()*10000000 ,10)
    window[functionName] = function(){  // 每次请求之前搞出一个随机的函数
        amount.innerText = amount.innerText - 0 - 1
    }
    script.src = '/pay?callback=' + functionName
    document.body.appendChild(script)
    script.onload = function(e){ // 状态码是 200~299 则表示成功
        e.currentTarget.remove()
        delete window[functionName] // 请求完了就kill掉这个随机函数
    }
    script.onload = function(e){ // 状态码大于等于 400 则表示失败
        e.currentTarget.remove()
        delete window[functionName] // 请求完了就kill掉这个随机函数
    }
})
```
```javascript
//后端代码
...
if (path === '/pay'){
    let amount = fs.readFileSync('./db', 'utf8')
    amount -= 1
    fs.writeFileSync('./db', amount)
    let callbackName = query.callback
    response.setHeader('Content-Type', 'application/javascript')
    response.write(`
        ${callbackName}.call(undefined, 'success')
    `)
    response.end()
}
...
```
### 4  用JSONP实现跨域
请求方：aaa.com 的前端程序员（浏览器）
响应方：bbb.com 的后端程序员（服务器）
假设有aaa.com、bbb.com 两个网站，aaa.com的前端想要访问bbb.com 的后端，aaa.com的前端可在请求的“/pay”前面加上bbb.com 的域名。通过跨域SRG。
后端不需要太了解前端的代码，如果太了解，就叫前端后端耦合，需要解耦。
解耦的方法：后段调用前端提供的一个函数。

### 5  JSONP 为什么不支持 POST？
因为JSONP是通过动态创建Script实现的，动态创建Script的时候只能用GET，不能用POST。