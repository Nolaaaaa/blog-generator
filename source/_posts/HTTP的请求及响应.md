---
title: HTTP的请求及响应
date: 2018-04-17 16:53:46
tags: HTTP
categories: 前端
---

HTTP是什么？HTTP 请求包括哪些部分？HTTP 响应包括哪些部分？如何使用 curl 命令？
<escape><!-- more --></escape>

### 1  HTTP是什么？

[HTTP](https://link.zhihu.com/?target=https%3A//zh.wikipedia.org/wiki/%25E8%25B6%2585%25E6%2596%2587%25E6%259C%25AC%25E4%25BC%25A0%25E8%25BE%2593%25E5%258D%258F%25E8%25AE%25AE) 全称：`HyperText Transfer Protocol`，即超文本传输协议HTTP的作用。

HTTP 作用：指导浏览器和服务器之间进行沟通。

### 2  HTTP 请求包括哪些部分？

HTTP请求主要包括四部分（第四部分可以为空），主要格式如下：
```
1 动词 路径 协议/版本
2 Key1: value1
2 Key2: value2
2 Key3: value3
2 Content-Type: application/x-www-form-urlencoded
2 Host: www.baidu.com
2 User-Agent: curl/7.54.0
3 
4 要上传的数据
```

### 3  HTTP 响应包括哪些部分？

HTTP响应同样包括四部分，主要格式如下：
```
1 协议/版本号 状态码 状态解释
2 Key1: value1
2 Key2: value2
2 Content-Length: 17931
2 Content-Type: text/html
3
4 要下载的内容
```

### 4  如何用Chrome开发者工具查看 HTTP 请求及响应的内容？

1. 首先进入chrome浏览器，`command+option+i `打开开发者页面。
2. 查看请求头部信息：打开一个网址，这里打开的是[https://st.hujiang.com](https://www.baidu.com/)，按照下图选择，如果不能看到请求的内容，点击request hearders(橙色的线条位置)旁边的的`view source`即可看到请求头。
3. 查看响应头部信息：点击`response hearders`(蓝色的线条位置)旁边的的`view source`，即可看到响应头。  
    ![avatar](https://i.loli.net/2018/06/03/5b12d6501ffab.png)
    
4. 查看响应的内容，点击Hearders 旁边的Preview即可，如下图：  
    ![avatar](https://i.loli.net/2018/06/03/5b12d6beb6358.png)

### 5  如何使用 curl 命令？

1. 什么是`curl`：`curl`是Linux下一个很强大的http命令行工具。
2. curl的基本用途：创造一个请求，并得到响应：
```javascript
curl -s -v -H "Nola: xxx" \-\- "https://www.baidu.com" 请求内容：
GET / HTTP/1.1 Host: www.baidu.com
User-Agent: curl/7.54.0 Accept: */* Nola: xxx
```
```javascript
curl -X POST -s -v -H "Nola: xxx" -- "https://www.baidu.com"
请求内容：
POST / HTTP/1.1
Host: www.baidu.com
User-Agent: curl/7.54.0
Accept: */* Nola: xxx
```  
```javascript
curl -X POST -d "1234567890" -s -v -H "Nola: xxx" \-\- "https://www.baidu.com" 
请求内容：
POST / HTTP/1.1 Host: www.baidu.com
User-Agent: curl/7.54.0 Accept: */* Nola: xxx
Content-Length: 10
Content-Type: application/x-www-form-urlencoded
    
1234567890
```