---
title: Hexo--Blog Beautify
date: 2018-04-03 22:19:17
tags: Hexo
categories: 备忘
---

这篇的目的是记录美化博客的过程
<escape><!-- more --></escape>
Ps：`$ hexo generate`和`$ hexo deploy`两句长长的语句可以简写成一句`$ hexo d -g`,使用一个内容为`<!-- more -->`的闭合`escape`标签可以让首页只展示这个标签前面的部分

本博客美化的内容有：
1 改变页面主题并添加动画
2 改变博客字体大小
3 给博客添加icon
4 添加作者的头像
5 去掉博客底部的Powered by
6 给文章添加分享功能
7 给博客添加评论功能
8 给博客添加阅读量

### 1 改变主题并添加动画
1. 改变主题，可在主题配置文件`_config.yml`中搜索`Scheme Settings`，四个主题中，去掉想要的主题前面的井号注释。Scheme 是 NexT 提供的一种特性，借助于 Scheme，NexT 为你提供多种不同的外观。同时，几乎所有的配置都可以 在 Scheme 之间共用。目前 NexT 支持三种 Scheme，其中:
  * Muse - 默认 Scheme，这是 NexT 最初的版本，黑白主调，大量留白
  * Mist - Muse 的紧凑版本，整洁有序的单栏外观
  * Pisces - 双栏 Scheme，小家碧玉似的清新
  * Gemini - 左侧网站信息及目录，块+片段结构布局 

2. 添加背景动画，可在主题配置文件`_config.yml`中搜索`canvas_nest`，然后在想要的动画后把false改成true
```javascript
canvas_nest: false
three_waves: false
canvas_lines: true
canvas_sphere: false
```

### 2 改变博客字体大小
1. 可以在`/themes/(主题名)/source/css/_variables/base.styl`中修改
2. 也可以在主题配置文件`_config.yml`中搜索`font`修改

### 3 给博客添加icon
1. 找到想要的图，把图片放在`/themes/(主题名)/source/images`路径中，并在主题配置文件`_config.yml`中搜`favicon `
```javascript
favicon:
  small: /images/icon.png
  medium: /images/icon.png
  apple_touch_icon: /images/icon.png
```
### 4 添加作者的头像
1. 找到想要的图，把图片放在`/themes/(主题名)/source/images`路径，并在主题配置文件`_config.yml`中搜`avatar `
`avatar: /images/head.jpg`

### 5 去掉博客底部的Powered by
1. 方法一：在主题配置文件`_config.yml`中搜索`theme`，把如下两个值改成false
```
theme:
  enable: false
  version: false
```
2. 方法二：用VScode打开如下路径下的文件，改变代码内容`/themes/(主题名)/layout/_partials/footer.swig`

### 6 给文章添加分享功能
1. 在主题配置文件`_config.yml`中搜索`share`
```javascript
needmoreshare2:
  enable: true
  postbottom:
    enable: ture
    options:
      iconStyle: default
      boxForm: horizontal
      position: bottom + Left
      networks: Weibo,Wechat,QQZone,Evernote,Twitter,Facebook
```

### 7 给博客添加评论功能
1. `进入leanCloud网站`——`注册`（校验邮箱）——`创建应用`（不用选什么直接点创建，当然有钱也可以点商用的，随便花，反正我没钱）
2. `设置`——`应用key`——复制`App ID & App Key`放到主题配置文件`_config.yml`中(搜索`valine`)（并设置`enable`为`true`）
```javascript
valine:
  enable: true
  appid:     
  appkey:   
```
3. `设置`——`安全域名`——在`Web 安全域名`处添加博客的域名	
4. 在hexo路径的终端中输入`npm install valine --save`
5. 可以去leanCloud管理控制台查看，`储存`——`comment`处可查看评论，也可以修改喔
		
### 8 给博客添加阅读量
1. 和添加评论功能使用同一个网站，在主题配置文件中的设置不同，主题配置文件`_config.yml`中搜索`leancloud_visitors`
```javascript
leancloud_visitors:
  enable: ture
  app_id: 
  app_key: 
```
