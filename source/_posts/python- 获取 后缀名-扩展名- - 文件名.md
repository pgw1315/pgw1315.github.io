---
title: python- 获取 后缀名(扩展名) / 文件名
author: Will Holmes
categories: Python
tags:
  - Python
  - 文件处理
date: 2021-11-15 12:43:57
---

## method
使用 `os.path.splitext(file)[0]` 可获得 **文件名** 。   
 使用 `os.path.splitext(file)[-1]` 可获得以 `.` 开头的 **文件后缀名** 。
## code
```python
import os
file = "Hello.py"
# 获取前缀（文件名称）
assert os.path.splitext(file)[0] == "Hello"
# 获取后缀（文件类型）
assert os.path.splitext(file)[-1] == ".py"
assert os.path.splitext(file)[-1][1:] == "py"
```
