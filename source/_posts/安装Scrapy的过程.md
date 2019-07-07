---
title: 安装 Scrapy 遇到的一些坑及处理
date: 2019-07-07 22:04:56
tags: [Python]
categories: 后端
---

记录了安装 Scrapy 的过程，及遇到的一些坑的处理
<escape><!-- more --></escape>
### 使用的电脑及 Python 版本
电脑：Mac，Python的版本：3.7.3

### 下载 Scrapy
在终端输入如下命令：
```bash
$ python3 -m pip install scrapy
```
遇到报错：`Building wheel for Twisted ( [setup.py](setup.py) ) ... error`
解决办法：下载 `Twisted`

### 下载 Twisted
下载地址：[Twisted](https://pypi.org/project/Twisted/#files)
下载 tar.bz2 格式文件后>>解压>>进入解压后的文件夹终>>运行如下命令进行安装：
```bash
$ python3 setup.py install
```
遇到报错：`error: command ‘gcc’ failed with exit status 1`
解决办法：使用 brew 重新下载 Python3 ，下载完成后重新执行**第2步**
