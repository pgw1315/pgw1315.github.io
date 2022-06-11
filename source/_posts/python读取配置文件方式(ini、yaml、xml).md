---
title: python读取配置文件方式(ini、yaml、xml)
date: 2022-06-11 14:40:27
author: Will
img: /images/banner/Python-Learning.webp
categories: Python
tags:
  - Python
---
        
## 前言
python代码中配置文件是必不可少的内容。常见的配置文件格式有很多中：ini、yaml、xml、properties、txt、py等。
## 一、ini文件
### 1.1 ini文件的格式
```ini
; 注释内容
 [url] ; section名称
 baidu = https://www.zalou.cn
 port = 80
 [email]
 sender = ‘xxx@qq.com’
```
注意section的名称不可以重复，注释用分号开头。
### 1.2 读取 configparser
python自带的configparser模块可以读取.ini文件，注意：在python2中是ConfigParser

创建文件的时候，只需要在pychrame中创建一个扩展名为.ini的文件即可。
```python
import configparser

file = 'config.ini'

# 创建配置文件对象
con = configparser.ConfigParser()

# 读取文件
con.read(file, encoding='utf-8')

# 获取所有section
sections = con.sections()
# ['url', 'email']


# 获取特定section
items = con.items('url') # 返回结果为元组
# [('baidu','https://www.zalou.cn'),('port', '80')] # 数字也默认读取为字符串

# 可以通过dict方法转换为字典
items = dict(items)
```

## 二、yaml配置文件

### 2.1 yaml文件格式
yaml文件是用来方便读写的一种格式。它实质上是一种通用的数据串行话格式。

它的基本语法如下：
+ 大小写敏感
+ 缩进表示层级关系
+ 缩进时不允许使用Tab，仅允许空格
+ 空格的多少不重要，关键是相同层级的元素要对齐
+ #表示注释，#后面的字符都会被忽略
  
yaml支持的数据格式包括:
+ 字典
+ 数组
+ 纯量：单个的，不可再次分割的值
 
### 2.1.2 对象
对象是一组组的键值对，使用冒号表示结构
```yaml
url: https://www.zalou.cn
log: 
 file_name: test.log
 backup_count: 5
```

yaml也允许另外一种写法，将所有的键值对写成一个行内对象
```yaml
 log: {file_name: test.log, backup_count: 5}
```
 
### 2.1.3 数组
一组横线开头的行，组成一个数组。
```yaml
 – cat
 – Dog
 – Goldfish
 ```
转换成python对象是
```python
 [‘cat’, ‘Dog’, ‘Goldfish’]
```
数组也可以采用行内写法：
```yaml
 animal: [cat, dog]
```
转行成python对象是
```python
 {‘animal’: [‘cat’, ‘dog’]}
```

### 2.1.4 纯量
纯量是最基本，不可分割的值。
数字和字符串直接书写即可：
```yaml
 number: 12.30
 name: zhangsan
```
布尔值用true和false表示
```yaml
 isSet: true
 flag: false
```
null用~表示
```yaml
 parent: ~
```
yaml允许用两个感叹号表示强制转换
```yaml
 e: !!str 123
 f: !!str true
```
 
### 2.1.5 引用
锚点&和别名*，可以用来引用
```yaml
defaults: &defaults
 adapter: postgres
 host: localhost
 
development: 
 databases: myapp_deveploment
 <<: *defaults

test:
 databases: myapp_test
 <<: *defaults
```

等同于以下代码

```yaml
defaults: 
 adapter: postgres
 host: localhost
 
development: 
 databases: myapp_deveploment
 adapter: postgres
 host: localhost

test:
 databases: myapp_test
 adapter: postgres
 host: localhost
```

&用来建立锚点（defaults），<<表示合并到当前数据，*用来引用锚点
下面是另外一个例子：
```yaml
 – &abc st
 – cat
 – dog
 – *abc
``` 
转换成python代码是：
```python
 [‘st’, ‘cat’, ‘dog’, ‘st’]
``` 
### 2.2 yaml文件的读取
读取yaml文件需要先安装相应模块。
```shell
pip install yaml
```
yaml文件内容如下：
```yaml
url: https://www.baidu.com
email:
 send: xxx@qq.com
 port: 25

---
url: http://www.sina.com.cn
```

读取代码如下：
```bash
# coding:utf-8
import yaml

# 获取yaml文件路径
yamlPath = 'config.yaml'

with open(yamlPath,'rb') as f:
 # yaml文件通过---分节，多个节组合成一个列表
 date = yaml.safe_load_all(f)
 # salf_load_all方法得到的是一个迭代器，需要使用list()方法转换为列表
 print(list(date))
```

## 三、xml配置文件读取
xml文件内容如下：
```xml
<collection shelf="New Arrivals" 
<movie title="Enemy Behind" 
 <type War, Thriller</type 
 <format DVD</format 
 <year 2003</year 
 <rating PG</rating 
 <stars 10</stars 
 <description Talk about a US-Japan war</description 
</movie 
<movie title="Transformers" 
 <type Anime, Science Fiction</type 
 <format DVD</format 
 <year 1989</year 
 <rating R</rating 
 <stars 8</stars 
 <description A schientific fiction</description 
</movie 
 <movie title="Trigun" 
 <type Anime, Action</type 
 <format DVD</format 
 <episodes 4</episodes 
 <rating PG</rating 
 <stars 10</stars 
 <description Vash the Stampede!</description 
</movie 
<movie title="Ishtar" 
 <type Comedy</type 
 <format VHS</format 
 <rating PG</rating 
 <stars 2</stars 
 <description Viewable boredom</description 
</movie 
</collection 
```

读取代码如下：
```python
# coding=utf-8
import xml.dom.minidom
from xml.dom.minidom import parse

DOMTree = parse('config.xml')
collection = DOMTree.documentElement
if collection.hasAttribute("shelf"):
 print("Root element : %s" % collection.getAttribute("shelf"))

# 在集合中获取所有电影
movies = collection.getElementsByTagName("movie")

# 打印每部电影的详细信息
for movie in movies:
 print("*****Movie*****")
 if movie.hasAttribute("title"):
  print("Title: %s" % movie.getAttribute("title"))

 type = movie.getElementsByTagName('type')[0]
 print("Type: %s" % type.childNodes[0].data)
 format = movie.getElementsByTagName('format')[0]
 print("Format: %s" % format.childNodes[0].data)
 rating = movie.getElementsByTagName('rating')[0]
 print("Rating: %s" % rating.childNodes[0].data)
 description = movie.getElementsByTagName('description')[0]
 print("Description: %s" % description.childNodes[0].data)
```

以上这篇python读取配置文件方式(ini、yaml、xml)就是小编分享给大家的全部内容了，希望能给大家一个参考。

## 参考文章
[python读取配置文件方式(ini、yaml、xml)](https://cloud.tencent.com/developer/article/1740527)