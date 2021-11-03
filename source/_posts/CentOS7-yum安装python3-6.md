---
title: CentOS7 yum安装python3.6
author: Will Holmes
categories: Linux
tags:
  - Linux
  - Python
date: 2021-11-03 10:42:26
---

> CentOS7 使用yum 安装Python3 

### 1.配epel源_阿里
```bash 
wget -O /etc/yum.repos.d/epel.repo http://mirrors.aliyun.com/repo/epel-7.repo
```
### 2.安装python3.6
```bash 
yum install python36 -y
```
### 3.安装pip3
```bash 
# 搜索pip3的安装包名称
yum  whatprovides pip3

# 安装pip3
yum install python36-pip -y
```
依赖：python36-setuptools

### 4.验证
```bash 
python3 --version
```
### 5.查看指向
```bash 
ll /usr/bin/python
ll /usr/bin/python3
```
由上可知
python 代表CentOS7系统默认的 python2.7；
python3 代表新安装的 python3.6
### 6.安装python开发工具
```bash 
yum install python36-devel -y
```

### 引用

[CentOS7 yum安装python3.6](https://blog.51cto.com/moerjinrong/2396290)