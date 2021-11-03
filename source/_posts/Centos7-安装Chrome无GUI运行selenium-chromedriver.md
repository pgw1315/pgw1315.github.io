---
title: Centos7 安装Chrome无GUI运行selenium chromedriver
author: Will Holmes
categories: Linux
tags:
  - Linux
  - Chrome
  - Python
  - Selenium
date: 2021-11-03 10:24:18
---

### 1. 安装chrome
首先安装google的epel源
```bash 
vi /etc/yum.repos.d/google.repo
```
``` 
[google]
name=Google-x86_64
baseurl=http://dl.google.com/linux/rpm/stable/x86_64
enabled=1
gpgcheck=0
gpgkey=https://dl-ssl.google.com/linux/linux_signing_key.pub 
```

```bash 
yum update && yum install google-chrome-stable
```


### 2.chromedriver下载
https://npm.taobao.org/mirrors/chromedriver/

找到chrome对应的chromedriver 版本，并下载
```bash 
wget https://chromedriver.storage.googleapis.com/74.0.3729.6/chromedriver_linux64.zip
```
将下载的chromedriver 放到脚本同级目录调用

### 3. 为chromedriver授权
```bash 
chmod 755 chromedriver
```

### 测试代码 
```python 
from selenium import webdriver
from selenium.webdriver.chrome.options import Options

chrome_options = Options()
chrome_options.add_argument("--headless")
chrome_options.add_argument('--disable-gpu')
chrome_options.add_argument('--no-sandbox')    # 禁止沙箱模式，否则肯能会报错遇到chrome异常
url="https://www.west.cn/login.asp"
brower=webdriver.Chrome(executable_path="./chromedriver", chrome_options=chrome_options)
brower.get(url)
print(brower.current_url)
brower.get("https://www.west.cn/Manager/")
print(brower.current_url)
brower.quit()
```

### 引用
[centos7无GUI运行selenium chromedriver 亲测可用！](https://www.cnblogs.com/huchong/p/10796823.html)

