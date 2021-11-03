---
title: 给Linux安装中文字体（解决Selenium截图出现乱码框）
author: Will Holmes
categories: Linux
tags:
  - Linux
  - Selenium
date: 2021-11-03 10:32:47
---
>用selenium做了一个网页截图的小功能，截出来的图片中有许多框框，这是因为linux缺少中文字体导致的。

### 下载字体
https://wwe.lanzoui.com/iKjPgw3109a

### 安装字体
```bash 
mkdir -p /usr/share/fonts/chinese/        #创建中文字体目录
cp songti.ttf /usr/share/fonts/chinese/     #将字体文件拷贝到/usr/share/fonts/chinese/中
cd /usr/share/fonts/chinese/
fc-cache -fv                              #为刚加入的字体设置缓存使之有效
fc-list                                   #查看系统中的字体
```

