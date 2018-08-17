---
title: 实现两个jQuery的API（addClass、text）
date: 2018-05-10 15:43:48
tags:
---

用原生JS实现jQuery的addClass和text两个API
<escape><!-- more --></escape>
### 目的
1. Quick Start给所有的div添加一个叫“red”的class，为方便看到代码的效果，设置如下css，在设置“red”成功时，文本会变红
```
.red{
  color:red;
}
```
2. 将所有的div中的textContent变为“Hi”，HTML代码如下：
```
<body>
  <div class="item1">选项1</div>
  <div class="item2">选项2</div>
  <div class="item3">选项3</div>
  <div class="item4">选项4</div>
  <div class="item5">选项5</div>
</body>
```
### 思路
完整代码及思路如下，效果 [点击这里](http://js.jirengu.com/supayifojo/7/edit?html,css,js,output) 
