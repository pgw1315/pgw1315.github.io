---
title: python获取当前文件所在目录和当前工作目录
date: 2022-06-20 12:05:06
author: Will
img: /images/banner/python-path.jpeg
categories: Python
tags:
  - Python
---

### 一、当前工作目录

```python
import  os
print(os.getcwd()) # 获取当前工作目录路径
print(os.path.abspath('.')) # 获取当前工作目录路径
```

### 二、当前文件路径

```python
import os

current_work_dir = os.path.dirname(__file__)  # 当前文件所在的目录

weight_path = os.path.join(current_work_dir, weight_path)  # 再加上它的相对路径，这样可以动态生成绝对路径
```

 
## 参考文章
[python获取当前文件所在目录和当前工作目录](https://blog.csdn.net/jizhidexiaoming/article/details/102930885)