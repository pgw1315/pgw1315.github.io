---
title: Python一秒钟搭建文件分享网页
date: 2022-06-10 12:09:28
author: Will
img: /images/banner/share.jpeg
categories: Python
tags:
  - Python
  - 文件
---
        
    Python的库是十分丰富的。在局域网内分享文件时，Python的http.server可以创建一个非常基本的Web服务器，相对于当前目录提供文件。http.server模块定义用于实现 HTTP 服务器（Web 服务器）的类。
###     一、运行简单网页

    先来试验一下，在cmd运行：python -m http.server，如果时linux平台的化，注意python版本3 。在浏览器输入IP地址:8000，即可获得的当前目录下的所有文件列表，以提供下载。需要注意的是防火墙要关闭或添加规则。
    在局域网内暂时分享文件的话是十分方便的，比U盘拷贝省心很多。

![](/images/Python一秒钟搭建文件分享网页/24447700-a76e1d33147b6118.png)

![](/images/Python一秒钟搭建文件分享网页/24447700-e9440b8b295e43e1.png)

#### 分享网页

查看官方文档：http请求映射到目录，检查目录中是否有index.html或index.htm文件（按顺序）。如果有，文件的内容将返回：否则，目录列表将通过调用list_directory()方法生成，此方法使用os.listdir()扫描目录。

### 二、http.server具体使用方法

 使用 python -m http.server --help 再来看一下具体的使用方法。

![](/images/Python一秒钟搭建文件分享网页/24447700-acbebe374b8d8c3c.png)


port指定端口号，而不是默认的8000。
--bind ADDRESS, -b ADDRESS此方式是针对多网卡设备，绑定特定的网卡可以访问，而不是全部网卡。
--directory DIRECTORY, -d DIRECTORY指定特定的目录，而不是当前目录。
下面的命令指定E:\download目录并通过端口8888运行http.server ，所有网卡组成的局域网IP地址都可以访问。
python -m http.server --directory E:\download 8888


![](/images/Python一秒钟搭建文件分享网页/24447700-a9f48c4af6cbc7aa.png)



   下面的命令指定E:\download目录并通过端口8888运行http.server ，特定网卡的局域网IP地址可以访问。    
    python -m http.server --directory E:\download --bind 192.168.15.219 8888

![](/images/Python一秒钟搭建文件分享网页/24447700-e5a3b5d7ac736f8e.png)

###     三、底层实现server.py

    上面的帮助信息里面有提示：usage: server.py [-h] [--cgi] [--bind ADDRESS] [--directory DIRECTORY] [port]  ，既是此种方法的底层调用的是server.py  （[github上也有托管](https://github.com%2Fpython%2Fcpython%2Ftree%2F3.3%2FLib%2Fhttp%2Fserver.py)）。下面笔者通过进入到server.py的目录，直接使用底层server.py来运行http服务，效果是一样的。

![](/images/Python一秒钟搭建文件分享网页/24447700-fd7af4ded14cafe7.png)
你也可以使用__pycache__目录下的已经编译好的.pyc文件。
![]/images/Python一秒钟搭建文件分享网页/24447700-2daa0e093f417a40.png)

###     四、Linux系统上实践
        在Llinux系统上，如果要远程拷贝文件可以使用scp命令，或者在windows上使用可视化程序winscp。笔者使用腾讯云服务器来测试（哈哈，忽略前面的登陆显示的狮子），需要注意的是腾讯云服务器密钥开启firewall防火墙，但是NAT转换服务器是有防火墙的，需要在控制台中开启TCP/8000端口。
    但是，官方不推荐在生产环境中使用 [http.server](https://docs.python.org%2Fzh-cn%2F3%2Flibrary%2Fhttp.server.html%23module-http.server) ，它只实现了基本的安全检查功能。所有临时搭建分享文件或者调试是可以的。

![](/images/Python一秒钟搭建文件分享网页/24447700-13b0f21eda21d9e8.png)

![](/images/Python一秒钟搭建文件分享网页/24447700-c31081ae3279e5d7.png)

## 参考文章
[Python一秒钟搭建文件分享网页](https://www.jianshu.com/p/f8c31bbbc1ef)