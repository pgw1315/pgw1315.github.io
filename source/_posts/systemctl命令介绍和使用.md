---
title: systemctl命令介绍和使用
date: 2022-06-06 09:25:00
author: Will
img: https://s1.ax1x.com/2022/05/17/O5806s.jpg
top: true
categories: Linux
tags:
  - Linux
  - 系统管理
---

### Systemd程序


Systemd其实是Linux系统用来管理系统的一个程序，用来代替原来的init进程(用来管理启动系统其它的服务进程)，现在很多Linux发行版都已经自带Systemd程序了。


### systemctl命令


#### 1. Unit


systemctl命令是Systemd中最重要的一个命令，用于对服务进行启动，停止等操作，在Systemd中有Unit的概念，每个进程都是一个Unit，总共有十二种Unit类型。


+ Service unit，系统服务
+ Target unit，多个 Unit 构成的一个组
+ Device Unit，硬件设备
+ Mount Unit，文件系统的挂载点
+ Automount Unit，自动挂载点
+ Path Unit，文件或路径
+ Scope Unit，不是由 Systemd 启动的外部进程
+ Slice Unit，进程组
+ Snapshot Unit，Systemd 快照，可以切回某个快照
+ Socket Unit，进程间通信的 socket
+ Swap Unit，swap 文件
+ Timer Unit，定时器



#### 2. 常用命令


```bash
# 列出正在运行的Unit
systemctl list-units，可以直接使用systemctl

# 列出所有Unit，包括没有找到配置文件的或者启动失败的
systemctl list-units --all

# 列出所有没有运行的 Unit
systemctl list-units --all --state=inactive

# 列出所有加载失败的 Unit
systemctl list-units --failed

# 列出所有正在运行的、类型为service的Unit
systemctl list-units --type=service

# 显示某个 Unit 是否正在运行
systemctl is-active application.service

# 显示某个 Unit 是否处于启动失败状态
systemctl is-failed application.service

# 显示某个 Unit 服务是否建立了启动链接
systemctl is-enabled application.service

# 立即启动一个服务
sudo systemctl start apache.service

# 立即停止一个服务
sudo systemctl stop apache.service

# 重启一个服务
sudo systemctl restart apache.service

# 重新加载一个服务的配置文件
sudo systemctl reload apache.service

# 重载所有修改过的配置文件
sudo systemctl daemon-reload
```



### systemctl中Unit的配置文件


上面说了每个服务都是一个Unit，那每个Unit都会有它的配置文件，这样启动的时候才知道要按照什么方式去启动。Systemd默认从目录`/etc/systemd/system/`读取配置文件。但是里面存放的大部分文件都是符号链接，指向目录`/usr/lib/systemd/system/`，真正的配置文件存放在那个目录。

#### 1. 查看Unit的配置文件


可以使用`systemctl cat`命令来查看服务的配置文件，下面是Mysql的配置文件，很多软件已经支持Systemd程序了，安装的时候会自动配置它的Unit配置文件，例如Mysql和Nginx等等。

```bash
[root@VM_0_11_centos ~]# systemctl cat mysqld
# /usr/lib/systemd/system/mysqld.service

[Unit]
Description=MySQL Server
Documentation=man:mysqld(8)
Documentation=http://dev.mysql.com/doc/refman/en/using-systemd.html
After=network.target
After=syslog.target

[Install]
WantedBy=multi-user.target

[Service]
User=mysql
Group=mysql
Type=forking
PIDFile=/var/run/mysqld/mysqld.pid
# Disable service start and stop timeout logic of systemd for mysqld service.
TimeoutSec=0
# Execute pre and post scripts as root
PermissionsStartOnly=true
# Needed to create system tables
ExecStartPre=/usr/bin/mysqld_pre_systemd
# Start main service
ExecStart=/usr/sbin/mysqld --daemonize --pid-file=/var/run/mysqld/mysqld.pid $MYSQLD_OPTS
# Use this to switch malloc implementation
EnvironmentFile=-/etc/sysconfig/mysql
# Sets open_files_limit
LimitNOFILE = 5000
Restart=on-failure
RestartPreventExitStatus=1
PrivateTmp=false


```


#### 2. Unit配置文件的含义


```bash

可以看到Unit配置文件有很多标签，不同的标签都代表了不同的意思，这里只列出部分介绍，可以去官网查看Unit配置文件文档介绍，https://www.freedesktop.org/software/systemd/man/systemd.unit.html。

- Unit
   - Description，服务的描述
   - Documentation，文档介绍
   - After，该服务要在什么服务启动之后启动，比如Mysql需要在network和syslog启动之后再启动
- Install
   - WantedBy，值是一个或多个Target，当前Unit激活时(enable)符号链接会放入/etc/systemd/system目录下面以Target名+.wants后缀构成的子目录中
   - RequiredBy，它的值是一个或多个Target，当前Unit激活(enable)时，符号链接会放入/etc/systemd/system目录下面以Target名+.required后缀构成的子目录中
   - Alias，当前Unit可用于启动的别名
   - Also，当前Unit激活(enable)时，会被同时激活的其他Unit
- Service
   - Type，定义启动时的进程行为。它有以下几种值。
   - Type=simple，默认值，执行ExecStart指定的命令，启动主进程
   - Type=forking，以 fork 方式从父进程创建子进程，创建后父进程会立即退出
   - Type=oneshot，一次性进程，Systemd 会等当前服务退出，再继续往下执行
   - Type=dbus，当前服务通过D-Bus启动
   - Type=notify，当前服务启动完毕，会通知Systemd，再继续往下执行
   - Type=idle，若有其他任务执行完毕，当前服务才会运行
   - ExecStart，启动当前服务的命令
   - ExecStartPre，启动当前服务之前执行的命令
   - ExecStartPost，启动当前服务之后执行的命令
   - ExecReload，重启当前服务时执行的命令
   - ExecStop，停止当前服务时执行的命令
   - ExecStopPost，停止当其服务之后执行的命令
   - RestartSec，自动重启当前服务间隔的秒数
   - Restart，定义何种情况 Systemd 会自动重启当前服务，可能的值包括always（总是重启）、on-success、on-failure、on-abnormal、on-abort、on-watchdog
   - TimeoutSec，定义 Systemd 停止当前服务之前等待的秒数
   - Environment，指定环境变量

```



### 自定义服务启动


既然Systemd的作用就是控制服务的启动，那么就可以把自己的服务添加进去，就可以直接使用systemctl命令来控制服务的启动，或者是设置开机自动启动等等。

#### 1. 创建Unit配置文件


在`/usr/lib/systemd/system`目录中创建自己的配置文件，一般都是`.service`结尾，例如这里创建了一个`test-sh.service`配置文件，这个Unit是为了启动我们自己的一个shell脚本。

```bash
# /usr/lib/systemd/system/test-sh.service
[Unit]
Description= test sh log

[Service]
ExecStart=/opt/dev/shell/test.sh
Type=forking
KillMode=process
Restart=on-failure
RestartSec=30s

[Install]
WantedBy=multi-user.target

```


#### 2. 创建脚本


```bash

在上面配置文件指定的启动路径`/opt/dev/shell/`下创建shell脚本，这里只是每秒打印当前时间，并输出到一个文本中。

#!/bin/bash
while true
do
sleep 1
 date=`date -d today +"%Y-%m-%d %T"`
 echo ${date} >> /opt/dev/shell/test.txt
done


```


#### 3. 载入配置文件并启动


```bash

使用`systemctl daemon-reload`命令来载入新添加的配置文件，然后使用`systemctl start test-sh.service`命令启动，再使用`systemctl status test-sh.service`命令来查看状态，可以看到已经启动，`/opt/dev/shell/test.txt`也确实在不停的写入内容，最后使用`systemctl stop test-sh.service`命令停止服务，可以看到状态也是停止了。

注意的是修改配置文件后一定要使用`systemctl daemon-reload`命令来载入新添加的配置文件，然后再启动服务。

[root@VM_0_11_centos ~]# systemctl start test-sh.service
^C
[root@VM_0_11_centos ~]# systemctl status test-sh.service
● test-sh.service - test sh log
   Loaded: loaded (/usr/lib/systemd/system/test-sh.service; enabled; vendor preset: disabled)
   Active: activating (start) since Fri 2020-06-26 05:46:45 CST; 11s ago
   Control: 9295 (test.sh)
   CGroup: /system.slice/test-sh.service
       ├─9295 /bin/bash /opt/dev/shell/test.sh
       └─9343 sleep 1

Jun 26 05:46:45 VM_0_11_centos systemd[1]: Starting test sh log...
[root@VM_0_11_centos ~]# systemctl stop test-sh.service
[root@VM_0_11_centos ~]# systemctl status test-sh.service
● test-sh.service - test sh log
   Loaded: loaded (/usr/lib/systemd/system/test-sh.service; enabled; vendor preset: disabled)
   Active: inactive (dead) since Fri 2020-06-26 05:47:52 CST; 2s ago
  Process: 9295 ExecStart=/opt/dev/shell/test.sh (code=killed, signal=TERM)

Jun 26 05:46:45 VM_0_11_centos systemd[1]: Starting test sh log...
Jun 26 05:47:52 VM_0_11_centos systemd[1]: Stopped test sh log.

```



### 查看Unit启动日志


Systemd统一管理了所有Unit的启动日志，因此只需要使用journalctl命令就可以查看到服务的日志

```bash
# 查看所有日志（默认情况下 ，只保存本次启动的日志）
journalctl

# 查看指定时间的日志
journalctl --since="2012-10-30 18:17:16"
journalctl --since "20 min ago"
journalctl --since yesterday
journalctl --since "2015-01-10" --until "2015-01-11 03:00"
journalctl --since 09:00 --until "1 hour ago"

# 显示尾部的最新10行日志
journalctl -n

# 显示尾部指定行数的日志
journalctl -n 20

# 实时滚动显示最新日志
journalctl -f

# 查看指定服务的日志
journalctl /usr/lib/systemd/systemd

# 查看指定进程的日志
journalctl _PID=1

# 查看某个路径的脚本的日志
journalctl /usr/bin/bash

# 查看指定用户的日志
journalctl _UID=33 --since today

# 查看某个 Unit 的日志
journalctl -u nginx.service
journalctl -u nginx.service --since today

# 实时滚动显示某个 Unit 的最新日志
journalctl -u nginx.service -f

# 合并显示多个 Unit 的日志
$ journalctl -u nginx.service -u php-fpm.service --since today

```

