---
title:  利用本地项目构建Chrome插件
date: 2019-03-28 20:52:57
tags: JavaScript
categories: 前端
---

最近在修改一个todoList小Demo，突然想到如果把它设置成 Chrome 插件并设置为 newtab 的默认页面，那么每次打开新页面就可以看到自己的 todo 啦，还可以提醒自己按时完成任务，于是就开始了尝试，并随手总结如下：
<escape><!-- more --></escape>

### 1 配置 manifest.json
把普通项目变成Chrome 插件的核心是配置一个名为 `manifest.json`的文件并添加到根目录中[点击差点配置文档](https://developer.chrome.com/extensions/manifest)
简单的`manifest.json`配置项如下：
```json
{
  // 插件名，必写
  "name": "Todo", 
  // 版本号，必写
  "version": "1.0", 
  // 清单文件的版本，必写，值为2
  "manifest_version": 2,
  // 描述
  "description": "My todo", 
  // 插件的图标，和 manifest.json 文件同一个目录下
  "browser_action": { 
  "default_icon": "icon.png"
  },
  // newtab的默认页面， manifest.json 文件同一个目录下
  "chrome_url_overrides": {
  "newtab": "index.html"
  },
  // 默认语言	
  "default_locale": "zh_CN",
}
```

### 2 浏览器添加插件
打开 Chrome -- 设置 -- 扩展程序 ，或者直接在地址栏输入：chrome://extensions
在扩展程序页面右上角勾选 开发者模式 -- 点击左上角的 加载正在开发的扩展程序 按钮 -- 选择扩展所在的文件夹，此时在浏览器工具栏中看到新添加的扩展。
在浏览器窗口新开一个标签页，此时新标签页初始显示的就是刚刚设置的导航页啦~~~~~~~

### 3 解决 vue.js 写的插件在浏览器打开一片空白的问题
**出现空白的原因Vue官方解释如下：：**
“有些环境，如 Google Chrome Apps，会强制应用内容安全策略[Content Security Policy (CSP)](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/CSP) ，不能使用 new Function() 对表达式求值。这时可以用 CSP 兼容版本。完整版本依赖于该功能来编译模板，所以无法在这些环境下使用。”
”另一方面，运行时版本则是完全兼容 CSP 的。当通过 `webpack + vue-loader` 或者 `Browserify + vueify` 构建时，模板将被预编译为 render 函数，可以在 CSP 环境中完美运行”

**解决方法：**
1 下载 csp 兼容版本的vue（最新的只有vue 1.x的）
2 使用安全策略方案，在`manifest.json`文件添加如下一行：
```json
{
  "content_security_policy": "script-src 'self' 'unsafe-eval'; object-src 'self';"
}
```

**实践项目地址：**
[普通项目地址](https://github.com/Nolaaaaa/navigation-page)
[vue项目地址](https://github.com/Nolaaaaa/todo)

ps: vue项目的`manifest.json`文件是放在打包文件dist目录里，设置插件的时候选择dist目录即可。
