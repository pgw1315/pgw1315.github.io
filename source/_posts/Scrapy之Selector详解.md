---
title: Scrapy之Selector详解
date: 2022-06-18 12:14:43
author: Will
img: /images/banner/python-scrapy.png
categories: Python
tags:
  - Python
  - Scrapy
---
        

## 一、简介

前面介绍了[scrapy命令](https://blog.csdn.net/trayvontang/article/details/103286889)和[Scrapy处理流程与重要组件 ](https://blog.csdn.net/trayvontang/article/details/103286971)

这里介绍一下Scrapy的Selector，Scrapy的Selector和Beautifulsoup非常像，关于Beautifulsoup可以参考[BeautifuSoup实用方法属性总结 ](https://blog.csdn.net/trayvontang/article/details/103263086)和[BeautifulSoup详解](https://blog.csdn.net/trayvontang/article/details/102811735)

先来看一下Selector的知识点：
+  
![Selector知识点](/images/Scrapy之Selector详解/20191128183547553.png)

## 二、xpath

我们先介绍一下xpath，因为xpath语法比较简洁，并且如果能够灵活应用的话，可以简化我们提取HTML内容的复杂度。

|符号|说明|
| --- |--- |
| / | 从根节点选取，使用绝对路径，路径必须完全匹配|
| // | 从整个文档中选取，使用相对路径|
| . | 从当前节点开始选取|
| … | 从当前节点父节点开始选取|
| @ | 选取属性|

光看说明有些抽象，我们通过一个例子来简单说明一下：

```python
# -*- coding:utf-8 -*-
from scrapy import Selector

content = '''
<div>
    <p>out inner div p</p>
    <div id="inner"><p>in inner div p</p></div>
</div>
<p>out div p</p>
'''

selector = Selector(text=content)

# 在整个文档中选取id为inner的div节点
inner_div_sel = selector.xpath("//div[@id='inner']")
# 获取整个文档中的p节点的文本
print(inner_div_sel.xpath('//p/text()').getall())
# 从inner div节点的父节点开始获取所有p节点的文本
print(inner_div_sel.xpath('..//p/text()').getall())
# 从inner div节点开始获取所有p节点的文本
print(inner_div_sel.xpath('.//p/text()').getall())

```

Scrapy的Selector和BeautifulSoup一样，可以通过字符串来构造相应的对象，然后就可以使用xpath相关的语法来解析HTML。

```python
inner_div_sel = selector.xpath("//div[@id='inner']")

```

首先@在xpath中表示选取属性，@id就表示选取id属性，//div[@id=‘inner’]就表示，选取id属性值为inner的div标签。

```python
inner_div_sel.xpath('//p/text()').getall()

```

上面的语句的输出是：

```python
['out inner div p', 'in inner div p', 'out div p']

```

很奇怪，我们已经在inner的div节点选取p，为什么获取到了所有的p？

**因为在xpath中，//表示从整个文档中选取，只要相对路径匹配就可以**

所有选取了文档中的所有p节点。

.和…就非常容易理解了，就是当前路径，和当前路径的上一级路径，路径必须完全匹配。

## 三、获取值

```python
# -*- coding:utf-8 -*-

from scrapy import Selector

content = '''
<html>
 <head>
  <title>selector css</title>
 </head>
 <body>
    <ul>
        <li>ul 1 li 1</li>
        <li>ul 1 li 2</li>
    </ul>
    <ul>
        <li>ul 2 li 1</li>
        <li>ul 2 li 2</li>
    </ul>
 </body>
</html>
'''

selector = Selector(text=content)

# get获取第一个
print(selector.css('li::text').get())
print(selector.css('li::text').extract_first())

# getall获取列表
print(selector.css('li::text').getall())
print(selector.css('li::text').c())

# ['<li>ul 1 li 1</li>', '<li>ul 2 li 1</li>']
print(selector.xpath('//li[1]').getall())
# ['<li>ul 1 li 1</li>']
print(selector.xpath('(//li)[1]').getall())

# 没找到返回None
print(selector.xpath('//div[@id="ne"]/text()').get())
# 给定默认值
print(selector.xpath('//div[@id="ne"]/text()').get(default='default-value'))

```

获取值使用get,getall,extract_first,extract方法，其中get与extract_first是获取第一个元素，getall和extract方法是获取所有元素，返回的是一个列表。

## 四、css选择

```python
# -*- coding:utf-8 -*-

from scrapy import Selector

content = '''
<html>
 <head>
  <title>attribute</title>
 </head>
 <body>
  <div id='images'>
   <a href='image1.html'>a text 1<img src='img1.png' /></a>
   <a href='image2.html'>a text 2<img src='img2.png' /></a>
   <a href='image3.html'>a text 3<img src='img3.png'>img3 text</img></a>
   <a href='image4.html'>a text 4<img /></a>
   <a href='http://mycollege.vip'>a text 5<img src='img5.jpg' /></a>
  </div>
 </body>
</html>
'''

selector = Selector(text=content)

# 获取第一个img标签的src属性
print(selector.css('img').attrib['src'])

# 获取所有a标签的href属性
print(selector.css('a::attr(href)').getall())
# 选择有href属性并且href属性中包含image的a标签
print(selector.css('a[href*=image]::attr(href)').getall())
# 选择href属性包含image的a标签下的属性包含src的标签
print(selector.css('a[href*=image] img::attr(src)').getall())

# 获取img的文本
print(selector.css('img::text').getall())
print(selector.css('img::text').get(default='default-value'))

# 获取id为imgages节点下的所有文本
print(selector.css('#images *::text').getall())
print(selector.css('title::text').get())

# xpath获取属性
print(selector.xpath('//a/@href').getall())
print(selector.xpath('//img/@src').getall())

```

Scrapy的css就是实现了css选择器，并且做了一定的扩展，可以更加方便的获取我们最常用的文本和属性。

## 五、xpath扩展

```python
# -*- coding:utf-8 -*-

"""
test()
starts-with()
contains()
"""

from scrapy import Selector

content = '''
<html>
 <head>
  <title>selector css</title>
 </head>
 <body>
    <ul>
        <li class="nav1"><a href="http://www.mycollege.vip"></a>nav1 text</li>
        <li class="nav2"><a href="http://www.mycollege.vip"></a>nav2 text</li>
        <li class="nav33"><a href="http://www.mycollege.vip"></a>nav33 text</li>
        <li class="clz1"><a href="http://www.mycollege.vip/xx.png"></a></li>
        <li class="clz1 clz2"><a href="https://www.mycollege.vip"></a></li>
    </ul>
 </body>
</html>
'''

selector = Selector(text=content, type="html")

# 选择class值匹配正则表达式nav\d$的li
print(selector.xpath(r'//li[re:test(@class, "nav\d$")]//text()').getall())

# 选择类包含clz1的li
print(selector.xpath('//li[has-class("clz1")]').getall())
# 选择类既包含clz1又包含clz2的li
print(selector.xpath('//li[has-class("clz1", "clz2")]').getall())
# 选择属性href包含png的a标签
print(selector.xpath('//a[contains(@href, "png")]/@href').getall())
# 选取href属性以https开头的a标签
print(selector.xpath('//a[starts-with(@href, "https")]/@href').getall())

```

对于xpath的扩展主要有判断是否包含指定类的has-class方法，属性是否包含指定字符串的contains方法，属性是否以指定字符串开头的starts-with方法。

## 参考文章
[Scrapy之Selector详解](https://blog.csdn.net/trayvontang/article/details/103298913)