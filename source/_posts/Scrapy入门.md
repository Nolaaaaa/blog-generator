---
title: Scrapy入门
date: 2019-07-08 22:48:56
tags: [Python]
categories: 后端
---

记录Scrapy入门的过程及中间遇到的一些坑
<escape><!-- more --></escape>

### 新建scrapy项目 test
```bash
$ scrapy startproject test
```

### 生成爬虫文件 test_spider.py
从终端进入test项目中的spiders文件夹，执行：
```bash
$ scrapy genspider test_spider test.com  # 最后两个参数：文件名 + 域名
```

### 明确要抓取的目标
在items.py里面编写，格式如：`name = scrapy.Field()`
```python
class TestItem(scrapy.Item):
  name = scrapy.Field()
```

### 编写爬虫文件
```python
class TestSpierSpider(scrapy.Spider):
  # 爬虫名字
  name = 'test_spider'   
  # 允许的域名
  allowed_domains = ['test.com']
  # 入口url，引擎会将url给调度器处理，调度器调度后又把请求给引擎，引擎把请求交给下载器下载解析
  start_urls = ['https://test.com']
  # 请求返回的数据解析
  def parse(self, response):
    print (response.text)

```

### 运行爬虫文件
```bash
$ scrapy crawl test_spider
```
**报错**：`DEBUG: Crawled (403) <GET https://test.com> (referer: None)`
**处理**：进入`settings.py`文件，找到`USER_AGENT`这个参数，从目标网站的请求头中复制`User-Agent`的值给`USER_AGENT`，再重新运行下爬虫文件即可

爬虫文件运行成功后即可看到response返回的html数据

运行爬虫文件更加简单的办法，test项目下新建一个文件`main.py`，再次运行爬虫文件无需进终端，运行此文件即可
```python
from scrapy import cmdline
cmdline.execute('scrapy crawl test_spider'.splite())
```
目前无法正常执行，寻找原因ing

### 爬虫文件中写xpath解析内容
可利用谷歌插件[XPath helper](https://chrome.google.com/webstore/search/XPath%20helper?hl=zh-CN)编写xpath
用xpath解析完了之后，需将数据yield到pipelines里面去才能正常导出数据

### 保存数据
导出数据存储为json文件：
```bash
$ scrapy crawl test_spider -o test.json
```
也可存为csv格式，后缀改一下就好了