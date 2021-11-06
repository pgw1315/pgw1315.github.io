---
title: CentOS使用yum安装ffmpeg工具
author: Will Holmes
categories: Linux
tags:
  - 软件
  - Linux
  - Yum
  - FFmpeg
date: 2021-11-07 04:28:07
---



## 第一种方法

### 安装ffmpeg
```bash 
yum -y install ffmpeg
```

### 安装yasm
```bash 
yum install -y yasm
```

### 查看是否安装成功
```bash 
ffmpeg -version
```
 

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200601162219387.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2d1b19xaWFuZ3FpYW5n,size_16,color_FFFFFF,t_70)

## 第二种方法

### 升级系统epel-release软件包

```bash
yum install epel-release -y //安装第三方软件包
yum update -y //更新系统软件包
shutdown -r now  //也可以不重启
  
```
**EPEL源-是什么?为什么安装？**

EPEL (Extra Packages for Enterprise
Linux)是基于Fedora的一个项目，为“红帽系”的操作系统提供额外的软件包，适用于RHEL、CentOS和Scientific Linux.

### 安装Nux Dextop Yum 源

由于CentOS没有官方FFmpeg rpm软件包。但是，我们可以使用第三方YUM源（Nux Dextop）完成此工作。

#### 1) CentOS 7

```bash

rpm --import http://li.nux.ro/download/nux/RPM-GPG-KEY-nux.ro
rpm -Uvh http://li.nux.ro/download/nux/dextop/el7/x86_64/nux-dextop-release-0-5.el7.nux.noarch.rpm
  
```
#### 2) CentOS 6

```bash

rpm --import http://li.nux.ro/download/nux/RPM-GPG-KEY-nux.ro
rpm -Uvh http://li.nux.ro/download/nux/dextop/el6/x86_64/nux-dextop-release-0-2.el6.nux.noarch.rpm
    
```
### 安装FFmpeg 和 FFmpeg开发包
```bash 
yum install ffmpeg ffmpeg-devel -y
```

### 测试是否安装成功
```bash 
ffmpeg 或 ffmpeg -version
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200601172835203.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2d1b19xaWFuZ3FpYW5n,size_16,color_FFFFFF,t_70)

### 如果你想了解更多关于FFmpeg使用方面的资料
```bash 
ffmpeg -h 
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200601172909584.png?size_16,color_FFFFFF,t_70)

