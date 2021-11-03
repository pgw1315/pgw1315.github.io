---
title: Python requests库session会话保存
author: Will Holmes
categories: Python
tags:
  - Python
  - Requests
date: 2021-10-17 23:03:03
---

> Python爬虫 requests库使用session会话保存cookies继续请求

```python 
import requests

# 通过Session类新建一个会话
session = requests.Session()
post_url = 'https://passport.weibo.cn/sso/login'
# 往下使用requests的地方，直接使用session即可，session就会保存服务器发送过来的cookie信息
headers = {
    'User-Agent': 'Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/63.0.3239.108 Safari/537.36',
    'Referer': 'https://passport.weibo.cn/signin/login?entry=mweibo&r=http%3A%2F%2Fweibo.cn%2F&backTitle=%CE%A2%B2%A9&vt=',

}
data = {
    'username': '17312345678', # 账号
    'password': 'password', # 密码
    'savestate': '1',
    'r': '',
    'ec': '0',
    'pagerefer': 'http://weibo.cn/',
    'entry': 'mweibo',
    'wentry': '',
    'loginfrom': '',
    'client_id': '',
    'code': '',
    'qq': '',
    'mainpageflag': '1',
    'hff': '',
    'hfp': '',
}

r = session.post(url=post_url, data=data, headers=headers)

# 上面的session会保存会话，往下发送请求，直接使用session即可
url = 'https://weibo.cn/6388179289/info'
headers1 = {
    'User-Agent': 'Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/63.0.3239.108 Safari/537.36',
}
r = session.get(url=url, headers=headers1)
print(r.text)

```


## 引用
原文链接：https://blog.csdn.net/haeasringnar/article/details/82558729