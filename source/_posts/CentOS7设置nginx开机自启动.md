---
title: CentOS7设置nginx开机自启动
author: Will Holmes
categories: Linux
tags:
  - Linux
  - 开机启动
  - Centos
date: 2021-11-21 01:19:17
---


> 服务器每次重启，都需要手动启动一些服务，这不是一个程序员可以忍受的，难怪大家都喜欢写脚本。接下来三篇文章，分别记录一下nginx、tomcat和mysql的开机自启动配置。

## Systemd

### Systemd简介
CentOS7已不再使用chkconfig管理启动项，而是使用systemd。关于systemd的衍生和发展，可以参见[《CentOS7/RHEL7 systemd详解》](https://www.linuxidc.com/Linux/2015-04/115937.htm)和[《CentOS7进程管理systemd详解》](https://www.linuxidc.com/Linux/2016-09/135464.htm)。简单介绍如下：
Linux系统从启动到提供服务的过程是这样，先是机器加电，然后通过MBR或者UEFI加载GRUB，再启动内核，内核启动服务，然后开始对外服务。
SysV init、UpStart、systemd主要是解决服务引导管理的问题。
SysV init是最早的解决方案，依靠划分不同的运行级别，启动不同的服务集，服务依靠脚本控制，并且是顺序执行的。在CentOS5中使用，配置文件为/etc/inittab。  
SysV init方案的优点是：原理简单，易于理解；依靠shell脚本控制，编写服务脚本门槛比较低。  
缺点是：服务顺序启动，启动过程比较慢；不能做到根据需要来启动服务，比如通常希望插入U盘的时候，再启动USB控制的服务，这样可以更好的节省系统资源。
为了解决系统服务的即插即用，UpStart应运而生，在CentOS6系统中，SysV init和UpStart是并存的，UpStart主要解决了服务的即插即用。服务顺序启动慢的问题，UpStart的解决办法是把相关的服务分组，组内的服务是顺序启动，组之间是并行启动。在CentOS6系统中，配置文件为/etc/inittab和/etc/init/*.conf。
但是随着移动互联网的到来，SysV init服务启动慢的问题显得越来越突出，许多移动设备都是基于Linux内核，比如安卓。移动设备启动比较频繁，每次启动都要等待服务顺序启动，显然难以接受，systemd就是为了解决这个问题诞生的。在CentOS7中使用，其配置文件为/usr/lib/systemd/system/ 和 /etc/systemd/system/ 中的文件。  
systemd的设计思路是：尽可能的快速启动服务；尽可能的减少系统资源占用。

### Systemd使用
在CentOS7中，systemctl命令主要负责控制systemd系统和服务管理器。基本取代了service和chkconfig命令，虽然service和chkconfig命令依然保留，但是据说已经被阉割过。
参考[《Centos7下的systemctl命令与service和chkconfig》](https://blog.csdn.net/cds86333774/article/details/51165361)，整理常用命令如下：
* `systemctl --version`，查看版本。
* `whereis systemctl`，查看位置。
* `systemctl list-unit-files`，列出所有可用单元（服务）。
* `systemctl list-units`，列出所有运行中的单元。
* `systemctl --failed`，列出所有失败的单元。
* `systemctl list-unit-files | grep enable`，查看自启动的软件。
* `systemctl is-enabled mysqld.service`，查看某个单元是否开机启动。
* `systemctl status mysqld.service`，查看某个单元的状态。
* `systemctl start mysqld.service`，启动某个单元。
* `systemctl restart mysqld.service`，重启某个单元。
* `systemctl stop mysqld.service`，停止某个单元。
* `systemctl daemon-reload`，修改了某个单元的配置文件后，重载配置文件。
* `systemctl reload mysqld.service`，重载某个单元。
* `systemctl enable mysqld.service`，设置开机自启动。
* `systemctl disable mysqld.service`，关闭开机自启动。
* `systemctl kill mysqld`，杀死单元。
