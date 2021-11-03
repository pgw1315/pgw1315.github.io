---
title: python 把\u开头的unicode转中文
author: Will Holmes
categories: Python
tags:
  - Python
  - unicode
date: 2021-10-17 22:53:04
---
> python3 把\u开头的unicode转中文

## python3 
```python 
  i.encode('utf-8').decode('unicode_escape')  #i='\u751F\u5316\u5371\u673A'
```

## python2 
```python 
i.encode('utf-8').decode('unicode_escape')  #i='\u751F\u5316\u5371\u673A'
i.decode('unicode-escape')
```

总结 unicode_escape可看作Unicode的反向编码，属于unicode存储到文本的过程中另一种存储方式

## 引用
链接：[https://www.jianshu.com/p/541b62a7d4e5](https://www.jianshu.com/p/541b62a7d4e5)
