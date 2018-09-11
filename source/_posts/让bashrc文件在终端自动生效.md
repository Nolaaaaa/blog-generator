---
title: 让.bashrc文件在终端自动生效
date: 2018-04-19 18:09:14
tags: 
categories: 备忘
---

修改了`.bashrc`文件，想在打开终端时默认路径变成桌面路径。
<escape><!-- more --></escape>
代码如下
```
cd ~/desktop
export PATH="/Users/nola/local:$PATH"
```
但是每次通过ssh打开终端都需要重新`source ～/.bashrc`一下，十分麻烦。
于是今天终于找到一个办法，就是在`.bash_profile`文件里重新引用一次`.bashrc`，添加的代码如下：
```
if test -f .bashrc ; then
source .bashrc
fi
```