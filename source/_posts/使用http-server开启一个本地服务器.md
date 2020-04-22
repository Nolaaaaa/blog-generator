---
title: 使用http-server开启一个本地服务器
date: 2018-05-12 20:09:58
tags: 
categories: 备忘
---

在写前端页面中，经常会在浏览器运行HTML页面，从本地文件夹中直接打开的一般都是`file`协议，当代码中存在`http`或`https`的链接时，HTML页面就无法正常打开，为了解决这种情况，需要在在本地开启一个本地的服务器
<escape><!-- more --></escape>
本文是利用`node.js`中的`http-server`，开启本地服务，步骤如下：

### 1 下载`node.js`
官网地址： [https://nodejs.org](https://nodejs.org/)  
下载完成后在命令行输入命令`$ node -v`以及`$ npm -v`检查版本，确认是否安装成功。

### 2 下载`http-server`
在终端输入：
`$ npm install http-server -g`

### 3 开启 `http-server`服务
终端进入目标文件夹，然后在终端输入：
```javascript
$ http-server -c-1   （⚠️只输入http-server的话，更新了代码后，页面不会同步更新）
Starting up http-server, serving ./
Available on:
  http://127.0.0.1:8080
  http://192.168.8.196:8080
Hit CTRL-C to stop the server
```

### 4 关闭 `http-server`服务
按快捷键`CTRL-C`
终端显示`^Chttp-server stopped.`即关闭服务成功。