---
title: Python 正则表达式笔记
author: Will Holmes
categories: Python
tags:
  - Python
  - 正则表达式
  - 笔记
date: 2021-10-10 20:50:38
---


# Python正则表达式
正则表达式是一个特殊的字符序列，它能帮助你方便的检查一个字符串是否与某种模式匹配。


## re.match函数
> re.match只从待匹配的字符串或文本的开头位置开始匹配，即如果匹配的字符串不在开头，而是在中间或结尾，则无法匹配就返回None！ 

### 语法：
```python 
re.match(pattern, string, flags=0)
```


### 参数：

- pattern : 匹配的正则表达式                     
- string  : 要匹配的字符串。                     
- flags   : 标志位，用于控制正则表达式的匹配方式 

### 结果：
> match方法在没有匹配的任何内容就返回None，如果有匹配的内容就返回一个Match对象

### 示例
```python 
import re

str = "aabbcceAAAe11442299aabCCbeedsffg"
print(re.match("aa", str).group())
# 输出结果： aa
print(re.match("AA", str))
# 输出结果： None
```

## re.search函数
>re.search 扫描整个字符串并返回第一个成功的匹配。

### 语法：
```python 
re.search(pattern, string, flags=0)
```
### 参数：
- pattern : 匹配的正则表达式                     
- string  : 要匹配的字符串。                     
- flags   : 标志位，用于控制正则表达式的匹配方式 
  
### 结果：
> search方法在没有匹配的任何内容就返回None，如果有匹配的内容就返回一个Match对象

### 示例
```python 
import re

str = "aabbcceAAAe11442299aabCCbeedsffg"
print(re.search("aa", str).group())
# 输出结果： aa
print(re.search("AA", str).group())
# 输出结果： AA
```

## re.match与re.search的区别
> re.match只匹配字符串的开始，如果字符串开始不符合正则表达式，则匹配失败，函数返回None；而re.search匹配整个字符串，直到找到一个匹配。


## 检索和替换
Python 的 re 模块提供了re.sub用于替换字符串中的匹配项。
### 语法：
```python
re.sub(pattern, repl, string, count=0, flags=0)
```
### 参数：

- pattern : 正则中的模式字符串。
- repl : 替换的字符串，也可为一个函数。
- string : 要被查找替换的原始字符串。
- count : 模式匹配后替换的最大次数，默认 0 表示替换所有的匹配。

### 返回结果
> 替换后的字符串

### 示例
```python 
import re

str = "aabbcceAAAe11442299aabCCbeedsffg"
# 将小写的bb替换为大写的BB
res=re.sub("bb",'BB',str)
# 输出结果为替换后的字符串
print(res)
# 输出结果为：aaBBcceAAAe11442299aabCCbeedsffg
```

## re.compile 函数
> compile 函数用于编译正则表达式，生成一个正则表达式（ Pattern ）对象，供 match() 和 search() 这两个函数使用。
### 语法：
```python 
  re.compile(pattern[, flags])
```

### 参数：
- pattern : 一个字符串形式的正则表达式
- flags : 可选，表示匹配模式，比如忽略大小写，多行模式等，具体参见Flags标识位

## pattern.findall 函数
> 在字符串中找到正则表达式所匹配的所有子串，并返回一个列表，如果没有找到匹配的，则返回空列表。
>注意： match 和 search 是匹配一次 findall 匹配所有。

### 语法：
```python 
findall(string[, pos[, endpos]])
```

### 参数：
- string : 待匹配的字符串。
- pos : 可选参数，指定字符串的起始位置，默认为 0。
- endpos : 可选参数，指定字符串的结束位置，默认为字符串的长度。

### 返回结果
> 返回一个列表，如果没有匹配到则返回一个空列表

### 示例
```python 
# -*- coding:UTF8 -*-
 
import re
 
pattern = re.compile(r'\d+')   # 查找数字
result1 = pattern.findall('runoob 123 google 456')
result2 = pattern.findall('run88oob123google456', 0, 10)
 
print(result1)
print(result2)
# 输出结果：
# ['123', '456']
# ['88', '12']
```
## re.finditer 函数
> 和 findall 类似，在字符串中找到正则表达式所匹配的所有子串，并把它们作为一个迭代器返回。

### 语法
```python 
re.finditer(pattern, string, flags=0)
```
### 参数

- pattern : 匹配的正则表达式                     
- string  : 要匹配的字符串。                     
- flags   : 标志位，用于控制正则表达式的匹配方式 

### 返回结果
> finditer函数如果匹配到内容则返回一个迭代器，如果没有匹配到则返回None

### 示例
```python 
# -*- coding: UTF-8 -*-
 
import re
 
it = re.finditer(r"\d+","12a32bc43jf3") 
for match in it: 
    print (match.group() )
# 输出结果：
# 12 
# 32 
# 43 
# 3
```

## re.split函数
> split 方法按照能够匹配的子串将字符串分割后返回列表，它的使用形式如下：

### 语法
```python 
re.split(pattern, string[, maxsplit=0, flags=0])
```

### 参数
- pattern：匹配的正则表达式
- string：要匹配的字符串。
- maxsplit：分隔次数，maxsplit=1 分隔一次，默认为 0，不限制次数。

### 返回结果
> 返回一个列表

### 示例
```python 
import re

str = "aa-bb-cceAAAe11442299aabCCbeedsffg"
res=re.split('-',str)
print(res)
# 运行结果
# ['aa', 'bb', 'cceAAAe11442299aabCCbeedsffg']
```

## Flags标志位

- re.I:使匹配对大小写不敏感                                          
- re.L:做本地化识别（locale-aware）匹配                               
- re.M:多行匹配，影响 ^ 和 $                                          
- re.S:使 . 匹配包括换行在内的所有字符                                
- re.U:根据Unicode字符集解析字符。这个标志影响 \w, \W, \b, \B.        
- re.X:该标志通过给予你更灵活的格式以便你将正则表达式写得更易于理解。 


## Match对象
> Match对象是一个匹配对象，正则表达式匹配到的内容都会存放着这个对象里面，如果没有匹配到任何的内容那就返回None

### Match对象的属性

- .string:待匹配的文本                        
- .re:匹配时使用的patter对象（正则表达式） 
- .pos:正则表达式搜索文本的开始位置        
- .endpos:正则表达式搜索文本的结束位置        

### Match对象的方法

- .group():获得匹配后的字符串              
- .groups():获得正则表达式中的分组列表           
- .start():匹配字符串在原始字符串的开始位置 
- .end():匹配字符串在原始字符串的结束位置 
- .span():返回(.start(), .end())          


## 参考链接
[Python中文文档-re正则表达式运算](https://www.docs4dev.com/docs/zh/python/3.7.2rc1/all/library-re.html)
[菜鸟教程-Python正则表达式](https://www.runoob.com/python/python-reg-expressions.html#flags)