---
title: Cookie和登录注册
date: 2018-08-19 20:34:10
tags:
---

为什么会存在Cookie？因为 [HTTP协议](https://zh.wikipedia.org/wiki/HTTP) 是无状态的，即 [服务器](https://zh.wikipedia.org/wiki/%E6%9C%8D%E5%8A%A1%E5%99%A8) 不知道用户上一次做了什么，这严重阻碍了 [交互式Web应用程序](https://zh.wikipedia.org/wiki/%E4%BA%A4%E4%BA%92%E5%BC%8FWeb%E5%BA%94%E7%94%A8%E7%A8%8B%E5%BA%8F) 的实现。
Cookie就是用来绕开HTTP的无状态性的“额外手段”之一。服务器可以设置或读取Cookies中包含信息，借此维护用户跟服务器 会话中的状态。
<escape><!-- more --></escape>
### Cookie 是什么？
参考自方方在知乎的回答[Cookie 是什么](https://zhuanlan.zhihu.com/p/22396872?refer=study-fe)
1. Cookie 是浏览器访问服务器后，服务器传给浏览器的一段数据。
2. 浏览器需要保存这段数据，不得轻易删除。
3. 此后每次浏览器访问该服务器，都必须带上这段数据。

### Cookie 作用是什么？
1. 识别用户身份
2. 记录历史

### Cookie的缺陷是什么？
1. Cookie会被附加在每个HTTP请求中，所以无形中增加了流量。
2. 由于在HTTP请求中的Cookie是明文传递的，所以存在安全性成问题，除非用 [HTTPS](https://zh.wikipedia.org/wiki/HTTPS) 。（Cookie会包含了一些敏感消息：用户名、计算机名、使用的浏览器和曾经访问的网站。）
3. Cookie的大小限制在4KB左右，对于复杂的存储需求来说是不够用的。

### Cookie替代？
在与服务器传输数据时，通过在地址后面添加唯一 [查询串](https://zh.wikipedia.org/w/index.php?title=%E6%9F%A5%E8%AF%A2%E4%B8%B2&action=edit&redlink=1) ，让服务器识别是否合法用户，也可以避免使用Cookie


待添加～～～


            

