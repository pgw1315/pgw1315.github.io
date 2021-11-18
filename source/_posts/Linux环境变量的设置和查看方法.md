---
title: Linux环境变量的设置和查看方法
author: Will Holmes
categories: Linux
tags:
	- Linux
	- 环境变量
date: 2021-11-17 10:06:03
---

# Linux环境变量的设置和查看方法
 
```bash
$ echo $HOME
/home/redbooks
```
1. 设置一个新的环境变量hello

```bash
$ export HELLO="Hello!"
$ echo $HELLO
Hello!
```
2. 使用env命令显示所有的环境变量
```bash
$ env
HOSTNAME=redbooks.safe.org
PVM\_RSH=/usr/bin/rsh
Shell=/bin/bash
TERM=xterm
HISTSIZE=1000
...
```
4. 使用set命令显示所有本地定义的Shell变量
```bash
$ set
BASH=/bin/bash
BASH\_VERSINFO=([0]="2"[1]="05b"[2]="0"[3]="1"[4]="release"[5]="i386-redhat-linux-gnu")
BASH\_VERSION='2.05b.0(1)-release'
COLORS=/etc/DIR\_COLORS.xterm
COLUMNS=80
DIRSTACK=()
DISPLAY=:0.0
...
```
5. 使用unset命令来清除环境变量
```bash
set可以设置某个环境变量的值。清除环境变量的值用unset命令。如果未指定值，则该变量值将被设为NULL。示例如下：
$ export TEST="Test..." #增加一个环境变量TEST
$ env|grep TEST #此命令有输入，证明环境变量TEST已经存在了
TEST=Test...
$ unset $TEST #删除环境变量TEST
$ env|grep TEST #此命令没有输出，证明环境变量TEST已经存在了
```
6. 使用readonly命令设置只读变量
```bash
如果使用了readonly命令的话，变量就不可以被修改或清除了。示例如下：
$ export TEST="Test..." #增加一个环境变量TEST
$ readonly TEST #将环境变量TEST设为只读
$ unset TEST #会发现此变量不能被删除
-bash: unset: TEST: cannot unset: readonly variable
$ TEST="New" #会发现此也变量不能被修改
-bash: TEST: readonly variable
环境变量的设置位于/etc/profile文件
如果需要增加新的环境变量可以添加下属行
export path=$path:/path1:/path2:/pahtN
```
　　-----------------------------------------------------------------------------------------------------------------------

## 　　1.Linux的变量种类
　　按变量的生存周期来划分，Linux变量可分为两类：

　1.1 永久的：需要修改配置文件，变量永久生效。

　1.2 临时的：使用export命令声明即可，变量在关闭shell时失效。
## 　　2.设置变量的三种方法
2.1 在/etc/profile文件中添加变量【对所有用户生效(永久的)】
　　用VI在文件/etc/profile文件中增加变量，该变量将会对Linux下所有用户有效，并且是“永久的”。

　　例如：编辑/etc/profile文件，添加CLASSPATH变量

　　# vi /etc/profile

　　export CLASSPATH=./JAVA\_HOME/lib;$JAVA\_HOME/jre/lib

　　注：修改文件后要想马上生效还要运行# source /etc/profile不然只能在下次重进此用户时生效。

2.2 在用户目录下的.bash\_profile文件中增加变量【对单一用户生效(永久的)】

　　用VI在用户目录下的.bash\_profile文件中增加变量，改变量仅会对当前用户有效，并且是“永久的”。

　　例如：编辑guok用户目录(/home/guok)下的.bash\_profile

　　$ vi /home/guok/.bash.profile

　　添加如下内容：

　　export CLASSPATH=./JAVA\_HOME/lib;$JAVA\_HOME/jre/lib

　　注：修改文件后要想马上生效还要运行$ source /home/guok/.bash\_profile不然只能在下次重进此用户时生效。
2.3 直接运行export命令定义变量【只对当前shell(BASH)有效(临时的)】

　　在shell的命令行下直接使用[export 变量名=变量值] 定义变量，该变量只在当前的shell(BASH)或其子shell(BASH)下是有效的，shell关闭了，变量也就失效了，再打开新shell时就没有这个变量，需要使用的话还需要重新定义。

## 　　3.环境变量的查看

3.1 使用echo命令查看单个环境变量。例如：
echo $PATH

3.2 使用env查看所有环境变量。例如：
	env
3.3 使用set查看所有本地定义的环境变量。

　　unset可以删除指定的环境变量。

### 　　4.常用的环境变量

　　PATH 决定了shell将到哪些目录中寻找命令或程序

　　HOME 当前用户主目录

　　HISTSIZE　历史记录数

　　LOGNAME 当前用户的登录名

　　HOSTNAME　指主机的名称

　　SHELL 　　当前用户Shell类型

　　LANGUGE 　语言相关的环境变量，多语言可以修改此环境变量

　　MAIL　　　当前用户的邮件存放目录

　　PS1　　　基本提示符，对于root用户是#，对于普通用户是$ 
