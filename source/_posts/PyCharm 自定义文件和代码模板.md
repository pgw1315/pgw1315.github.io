---
title: PyCharm 自定义文件和代码模板
author: Will Holmes
categories: 工具
tags:
 - PyCharm
date: 2021-11-23 09:49:17
---

　　PyCharm提供了文件和代码模板功能，可以利用此模板来快捷新建代码或文件。比如在PyCharm中新建一个html文件，新的文件并不是空的，而是会自动填充了一些基础的必备的内容，就像这样:
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
</body>
</html>
```
　　系统自带的模板内容可能并不是想要的，自己可以修改增加个性化的内容，比如我新建一个名为main.py的Python文件，会自动填充这些内容:
```python
# !/usr/bin/env python3
# -*- encoding: utf-8 -*-
'''
@项目 :  ${PROJECT_NAME}
@文件 :  ${NAME}.py
@时间 :  ${YEAR}/${MONTH}/${DAY} ${HOUR}:${MINUTE}
@作者 :  ${USER}
@版本 :  1.0
@说明 :   

'''
```
　　File Name为文件名， Author是登录系统的用户名, 日期为当前系统日期。是不是感觉比默认的空白文件好多了。具体的修改步骤是: 【文件(File)】 → 【设置(Settings)】如图操作, 在【编辑器(Editor)】中找到【文件和代码模板(File and Code Templates)】，选择你想要设置的文件类型进行编辑即可。
![](https://images2015.cnblogs.com/blog/1039209/201706/1039209-20170605125127575-2038114546.png)
　　
　　附上模板变量:
```
 ${PROJECT_NAME} - 当前Project名称;
 ${NAME} - 在创建文件的对话框中指定的文件名;
 ${USER} - 当前用户名;
 ${DATE} - 当前系统日期;
 ${TIME} - 当前系统时间;
 ${YEAR} - 年;
 ${MONTH} - 月;
 ${DAY} - 日;
 ${HOUR} - 小时;
 ${MINUTE} - 分钟；
 ${PRODUCT_NAME} - 创建文件的IDE名称;
 ${MONTH_NAME_SHORT} - 英文月份缩写, 如: Jan, Feb, etc;
 ${MONTH_NAME_FULL} - 英文月份全称, 如: January, February, etc；
```

