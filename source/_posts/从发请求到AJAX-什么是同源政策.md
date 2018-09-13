---
title: 从发请求到AJAX + 什么是同源政策
date: 2018-06-2 14:33:48
tags: [AJAX,同源政策]
categories: 前端
---

用form、a、image、link、script等标签都可以发请求，但是各有各的缺陷，这个时候就需要强大的AJAX出场了
<escape><!-- more --></escape>
### 1  发请求的各种方法

1. 使用form标签（会在当前页面刷新或者新开一个页面刷新）
```html
<form action="" method=post/get>
    <input type="submit">
</form>
```
    
2. 使用a标签（会在当前页面刷新或者新开一个页面刷新）
```html
    <a id=x href="">click</a>
    //让浏览器帮你自动点击
    <script>x.click</script>
```
    
3. 使用image标签（只能以图片的形式展示）
4. 使用link标签（只能以 CSS、favicon 的形式展示）
```html
<script>
    var link = document.createElement('link')
    link.rel = 'stylesheet' link.href = ' '
    //必须要放到head中
    document.head.appendChild(link) 
</script>
```
    
5. 使用script标签（只能以脚本的形式运行）
```html
<script>
    var script = document.createElement('script')
    script.src = ' '
    //必须要放到head或者body中
    document.head.appendChild(script) 
</script>
```
    
6. 等其他几种

### 2  新的请求方式AJAX

1. AJAX的产生
	*   以上发请求的各种方法都有缺点，那么有什么方式可以实现无论什么请求都行，想用什么形式展示就用什么形式展示？
	*   IE5在JS中引入activeX对象（API），使 JS 可以直接发起 HTTP 请求（IE从IE6就开始骄傲堕落了。。）
	*   随后 Mozilla、 Safari、 Opera 也跟进（抄袭）了，取名 XMLHttpRequest，并被纳入 W3C 规范（AJAX奠定了前端的基础）

2.  什么是AJAX？
	*   AJAX（异步的JavaScript和XML）四个字母的含义（A：异步 J：JavaScript A：and X：XML）
	*   AJAX需要满足三个条件
	    *   使用 XMLHttpRequest 发请求　　　　　　
	    *   服务器返回 XML 格式的字符串  　　　　　　
	    *   JS 解析 XML，并更新局部页面（AJAX默认异步)　　　　　
	*   简单说就是用JS发请求用JS处理响应　　
	
	*   XML格式的字符串太麻烦了，目前使用JSON
    

3. 什么是JSON？
	*   JavaScript Object Notation, JS 对象简谱，是一种轻量级的数据交换格式（抄袭JS的一门语言，是一门数据格式化语言）　　
	*   和JS相比数据结构中没有抄袭undefined、function、symbol　　
	    
	*   和JS相比，JSON的字符串的首尾必须是双引号，格式如{""}（⚠️：JSON中用的都是双引号，没有单引号）　　

4. 写一个AJAX
```
    myButton.addEventListener('onclick',function(){
        let request = new XMLHttpRequest()
        request.open('GET','/xxx') //想怎么请求就怎么请求
     request.send()
        request.onreadystatechange=function(){ if(request.readyState === 4){
                console.log('请求响应完毕了') if(request.status >= 200 && request.status < 300){ //console.log('说明请求成功')
                    //console.log(request.responseText)
                    let string = request.responseText //把符合JSON语法的字符串，转化成JS对应的值
                    //JSON.parse是浏览器提供的。document.getElementById也是浏览器提供的(也有很久以前用js自己写过叫json3.js)
                    let object = window.JSON.parse(string) //console.log(object)
                }else if(request.status >= 400){ //console.log('说明请求失败')
     }
            }
        }
    }) //题外话，怎么看一句执行用了多长的时间？ //console.time(); //var a=1; //console.timeEnd();
    
    //readyState的五种状态是（0、1、2、3、4） //如果状态是4，表示整个请求过程已经完毕
```
    
5. 用AJAX设置请求的四个部分
```javascript
let request = new XMLHttpRequest() //第一部分：open('请求的方式'，'请求协议&Host')
request.open('GET','/xxx') //想怎么请求就怎么请求

//第二部分：request.setRequestHeader('设置的类型','设置的内容')
request.setRequestHeader('Content-Type','x-www-form-urlencoded') //第三部分是空格不用设置

//第四部分：request.send('第四部分内容')
request.send('第四部分')

request.onreadystatechange=function(){}
```
    
5. 用AJAX获取响应的四个部分
```javascript
request.onreadystatechange=function(){ if(request.readyState === 4){
    console.log('请求响应完毕了') //响应的第一部分获取
    console.log(request.status)  //200
    console.log(request.statusText)  //OK

    if(request.status >= 200 && request.status < 300){ console.log('说明请求成功')

        //所有响应的Header获取
        console.log(request.getAllResponseHeaders()) //响应的第二部分获取
        console.log(request.getResponseHeader(Content-Type)) //响应的第三部分是空格

        //响应的第四部分获取
        console.log(request.responseText)

    }
}
```
    
6. 响应的四个部分是在服务器的node.js里设置的
    
```javascript
//如下为server.js中的代码 //path==='你的路径'
if(path === '/'){ //根据路径造一个字符串
    let string = fs.readFileSync('./index.html', 'utf8') //设置响应的第一部分。statusCode，200/400
    response.statusCode = 200
    //设置响应的第二部分
    response.setHeader('Content-Type', 'text/html;charset=utf-8') //设置响应的第四部分
    response.write(string) //然后结束
    response.end()
}
```
    

### 3  同源政策

1. 同源政策简单说就是：
是浏览器安全的基石（只要你不是不是某个页面的JS，就不能向这个页面发起AJAX请求（除了AJAX，其他的请求都可以））

2. 起源：
1995年由 Netscape 公司引入浏览器。目前，所有浏览器都实行这个政策
3. 目的：
是为了保证用户信息的安全，防止恶意的网站窃取数据。（因为AJAX可以读取浏览器响应的内容，如果没有同源政策的限制，就可以随便get、post，则互联网即没有隐私可言）　　

4. 同源政策的内容：
"同源"即"三个相同"，只有 协议+端口+域名 一模一样才允许发 AJAX 请求;如：http://baidu.com 不可以向 http://www.baidu.com 发请求。如http://baidu.com:80 不可以向 http://baidu.com:81 发请求）
随着互联网的发展，"同源政策"越来越严格。目前，如果非同源，共有三种行为受到限制。
    *   Cookie、LocalStorage 和 IndexDB 无法读取
    *   DOM 无法获得
    *   AJAX 请求不能发送。

### 4 如何规避同源

向另一个协议+端口+域名不一样的网页发起请求
方法一：用JSONP （不能post）　　
方法二：用CORS（全称：跨源资源共享 Cross-Origin Resource sharing）在后台加一句允许http://xxxx这个网站请求，如：response.setHeader('Access-Control-Allow-Origin','http://xxxx'）
方法三：window.postMessage[window.postMessage - Web API 接口 | MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/postMessage)

