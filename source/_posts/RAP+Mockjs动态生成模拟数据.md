---
title: RAP + Mock.js动态生成模拟数据
date: 2018-06-25 20:12:45
tags: [Mock.js,RAP]
categories: 前端
---

ps：补充一下了Mock.js的用法，以RAP不能使用的时候自己通过Mock的方式来处理接口
<escape><!-- more --></escape>
### 1 npm init
1.  `npm init`是用来装`package.json`的
2.  `npm init --yes`安装一个默认的`package.json`
3.  在安装一个要打包到生产环境的安装包时，你应该使用`npm install —save`，如果你在安装一个用于开发环境的安装包（例如，linter, 测试库等），你应该使用`npm install —save-dev`。
4.  如使用如下代码，则会自动在文档中添加一个`dependencies`模块（这些包在生产中需要）
```
$ npm install mockjs -S
或者
$ npm install mockjs --save 
```
5.  如使用如下代码，则会自动在文档中添加一个`devDependencies`模块（这些包用于开发和测试）
```
//安装到你项目的目录
$ npm install webpack -D
//全局安装  不建议用
$ npm install -g webpack  
```

### 2 RAP
1.  RAP 是一个 GUI （可视化）API管理工具，通过分析接口结构，动态生成模拟数据，校验真实接口正确性， 围绕接口定义，通过一系列自动化工具提升协作效率。在 RAP 中，可以定义接口的 URL、请求 & 响应细节格式等等。还提供 MOCK 服务、测试服务等工具，帮助开发团队提高开发效率。[RAP使用手册](https://github.com/thx/RAP/wiki/user_manual_cn)
2.  API是什么？
即Application Programming Interface，应用程序编程接口
3.  API管理工具是什么？
在前后端分离的开发模式下，为了方便前后端之间接口的展现和调用，提高开发效率，为了让测试人员更好的根据接口文档进行测试，通常需要定义一份API接口文档来规范接口的具体信息，如一个请求的地址、参数、参数名称及类型含义等等。 
API管理工具可以帮助我们管理这些接口，现在常用 API 管理工具有 Swagger、RAP、NEI、eolinker、EasyAPI、SosoApi、Postman 等。

### 3 Mock.js
1.  Mock.js 用于生成随机数据，拦截 Ajax 请求。[Mock.js示例](http://mockjs.com/examples.html)
2.  当RAP的接口不能使用的时候要怎么处理？
```
//下载Mock，并在页面引入Mock
import Mock from mock.js
let Random = Mock.Random
let data = Mock.mock({
  "lists|6": [
    {
      "id|10000-99999": 1,
      "img": "@image(178x178,@color)",
      "name": "@ctitle",
      "price|1-100.2-2": 1
    }
  ]
})
```
