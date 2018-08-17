---
title: 使用终端改变MAC默认截图存放地址
date: 2018-04-21 16:26:50
tags:
---

使用终端改变MAC默认截图存放地址的过程主要分为两步：
<escape><!-- more --></escape>
### 第一步：输入如下命令，回车
```
defaults write com.apple.screencapture location 要存放到的位置的绝对路径       
```
### 第二步：输入如下命令，应用刚才的操作
```
killall SystemUIServer
```
### 操作代码如下图：
[img](https://i.loli.net/2018/08/17/5b7686f84d0d0.png)