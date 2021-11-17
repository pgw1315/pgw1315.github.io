---
title: Python3—UnicodeEncodeError 'ascii' codec can't encode characters in position 0-1
author: Will Holmes
categories: Python
tags:
  - python
  - 编码
  - ascii

date: 2021-11-15 12:05:01
---

### 环境
```bash
>>> import sys
>>> print(sys.version)
'3.6.0 |Anaconda 4.3.1 (64-bit)| (default, Dec 23 2016, 12:22:00) \n[GCC 4.4.7 20120313 (Red Hat 4.4.7-1)]'
```
### 问题描述
今天在使用`python3`的时候，报错信息
```bash
Traceback (most recent call last):
  File "tmp.py", line 3, in <module>
    print(a)
UnicodeEncodeError: 'ascii' codec can't encode characters in position 0-1: ordinal not in range(128)
```
报错代码可简化为
```python
a = b'\xe5\x94\xb1\xe6\xad\x8c'
a = a.decode("utf-8")
print(a)
```
### 问题分析
本节介绍问题的分析过程，如果想看解决办法，可以直接看一下节。
#### 网上解释
网上给出的解释：错误的使用decode和encode方法会出现这种异常。例如使用decode方法将Unicode字符串转化的时候：
```python
s = u'中文'
s.decode('utf-8')
print s
```
但是将这个例子放到`python3`环境中，会报错
```bash
Traceback (most recent call last):
  File "tmp\_2.py", line 4, in <module>
    s.decode('utf-8')
AttributeError: 'str' object has no attribute 'decode'
```
熟悉python历史的朋友会知道，为了解决编码问题，在`python3`中，所有的字符串都是使用Unicode编码，统一使用`str`类型来保存，而str类型没有`decode`方法，所以网上给出的方向并不适合我的问题。
#### 字符编码
为了确定是否是字符编码的问题，我换了一台`python3`机器，测试了一下
```bash
>>>a = b'\xe5\x94\xb1\xe6\xad\x8c'
>>>a = a.decode("utf-8")
>>>print(a)
唱歌
```
完全没有问题，正常输出，排除字符编码和代码失误。
#### 输出
既然字符编码、代码都没有错，那么问题肯定出在`print`上面。这时我开始关注错误信息中的`ascii`。因为在一般`python3`环境中，输出时会将`Unicode`转化为`utf-8`。为了解开这个疑惑，查看了输出编码
```bash
>>>import sys
>>>sys.stdout.encoding
'ANSI\_X3.4-1968'
```
竟然是`ANSI_X3.4-1968`，所以任何中文都会报错。哈哈，终于定位问题啦。
### 解决方案
定位问题后，解决办法就很简单啦，有两种方法
* 使用[PYTHONIOENCODING](https://docs.python.org/3.6/using/cmdline.html#envvar-PYTHONIOENCODING)
运行python的时候加上PYTHONIOENCODING=utf-8，即
```bash
PYTHONIOENCODING=utf-8 python your\_script.py
```
* 重新定义标准输出
标准输出的定义如下
```bash
sys.stdout = codecs.getwriter("utf-8")(sys.stdout.detach())
```
打印日志的方法
```bash
sys.stdout.write("Your content....")
```
### 总结
通过分析这个问题，进一步加深了对python3的了解。另外，希望各位看官批评指正！！
