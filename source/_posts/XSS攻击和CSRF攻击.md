---
title: XSS攻击和CSRF攻击
date: 2018-07-20 14:36:42
tags: [XSS,CSRF,前端安全]
categories: 前端
---

整理了XSS攻击和CSRF攻击的攻击原理以及如何预防这类攻击
<escape><!-- more --></escape>
### 1  什么是XSS？怎么预防？
#### XSS是什么
**XSS**（Cross SiteScript跨站脚本攻击）
**攻击原理：**攻击者向有XSS漏洞的网站中输入(传入)恶意的HTML代码，当其它用户浏览该网站时，这段HTML代码会自动执行，从而达到攻击的目的。
#### XSS的风险
会话劫持，信息泄露
#### 预防
1.  使用innerText替代innerHTML
2. 使用 innerHTML 时进行字符过滤
    * 把 < 替换成 `&lt`
    * 把 > 替换成 `&gt`
    * 把 & 替换成 `&amp`
    * 把 ' 替换成 `&#39`
    * 把 ' 替换成 `&quot`
3. 使用 CSP （Content Security Policy内容安全性政策）

### 2  什么是CSRF？怎么预防？
#### CSRF是什么
**CSRF**（Cross-site request forgery跨站请求伪造）
**攻击原理：**
1. 用户C打开浏览器，访问受信任网站A，输入用户名和密码请求登录网站A；
2. 在用户信息通过验证后，网站A产生Cookie信息并返回给浏览器，此时用户登录网站A成功，可以正常发送请求到网站A；
3. 用户未退出网站A之前，在同一浏览器中，打开一个TAB页访问网站B；
4. 网站B接收到用户请求后，返回一些攻击性代码，并发出一个请求要求访问第三方站点A；
5. 浏览器在接收到这些攻击性代码后，根据网站B的请求，在用户不知情的情况下携带Cookie信息，向网站A发出请求。网站A并不知道该请求其实是由B发起的，所以会根据用户C的Cookie信息以C的权限处理该请求，导致来自网站B的恶意代码被执行。

#### 预防
1. 验证 HTTP Referer 字段
Referer 是HTTP 头中的一个字段，它记录了该 HTTP 请求的来源地址
2. 在请求地址中添加 token 并验证
在 HTTP 请求中以参数的形式加入一个随机产生的 token，并在服务器端建立一个拦截器来验证这个 token，如果请求中没有 token 或者 token 内容不正确，则认为可能是 CSRF 攻击而拒绝该请求
3. 在 HTTP 头中自定义属性并验证
token放到 HTTP 头中自定义的属性里（通过XMLHttpRequest）
4. 重要数据交互采用POST进行接收
当然是用POST也不是万能的，伪造一个form表单即可破解
