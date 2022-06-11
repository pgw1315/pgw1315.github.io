
---
title: Linux用netstat查看服务及监听端口详解
date: 2022-06-07 13:12:58
author: Will
img: https://s1.ax1x.com/2022/05/17/Ohh7kT.jpg
categories: Linux
tags:
  - Linux
  - 网络
---
        

在Linux使用过程中，需要了解当前系统开放了哪些端口，并且要查看开放这些端口的具体进程和用户，可以通过netstat命令进行简单查询

### netstat命令各个参数说明如下：


+ -a   或–all                             显示所有连线中的Socket。
+  -A                                       <网络类型>或–<网络类型> 列出该网络类型连线中的相关地址。
+  -c   或–continuous               持续列出网络状态。
+  -C 或–cache                       显示路由器配置的快取信息。
+  -e  或–extend                     显示网络其他相关信息。
+  -F  或 –fib                          显示FIB。
+  -g  或–groups                     显示多重广播功能群组组员名单。
+  -h  或–help                        在线帮助。
+  -i   或–interfaces                 显示网络界面信息表单。
+  -l  或–listening                    显示监控中的服务器的Socket。
+  -M   或–masquerade           显示伪装的网络连线。
+  -n  或–numeric                   直接使用IP地址，而不通过域名服务器。
+  -N   或–netlink或–symbolic  显示网络硬件外围设备的符号连接名称。
+  -o  或–timers                      显示计时器。
+  -p   或–programs                显示正在使用Socket的程序识别码和程序名称。
+  -r  或–route                        显示 Routing Table。
+  -s  或–statistice 显示网络工作信息统计表。
+  -t  或–tcp 显示TCP 传输协议的连线状况。
+  -u或–udp 显示UDP传输协议的连线状况。
+  -v或–verbose 显示指令执行过程。
+  -V 或–version 显示版本信息。
+  -w或–raw 显示RAW传输协议的连线状况。
+  -x或–unix 此参数的效果和指定”-A unix”参数相同。
+  –ip或–inet 此参数的效果和指定”-A inet”参数相同。


即可显示当前服务器上所有端口及进程服务，于grep结合可查看某个具体端口及服务情况：

```bash
[root@localhost ~]# netstat -ntlp   //查看当前所有tcp端口·
[root@localhost ~]# netstat -ntulp |grep 80   //查看所有80端口使用情况·
[root@localhost ~]# netstat -an | grep 3306   //查看所有3306端口使用情况·

[root@localhost ~]# netstat -nlp |grep LISTEN   //查看当前所有监听端口·
```


### 查看当前所有tcp端口使用情况：

![](/images/Linux用netstat查看服务及监听端口详解/1654944231.708832.jpg)

这里解释一下：

+ 1、0.0.0.0代表本机上可用的任意地址。 比如0.0.0.0:135 表示本机上所有地址的135端口，这样多ip计算机就不用重复显示了。
+  2、TCP 0.0.0.0:80表示在所有的可用接口上监听TCP80端口 
+  3、0.0.0.0为默认路由，即要到达不再路由表里面的网段的包都走0.0.0.0这条规则

然后127.0.0.1就是表示你本机ip地址的意思了。

然后[::]:21这又是什么鬼？

这个表示ipv6的21号端口的意思。

还有UDP的外部链接怎么都是*:*呢？

*：*是网址的通配符，就是192.168.15.12，这个类型的整体描述。

解释一下状态（state）了


+ LISTEN：(Listening for a connection.)侦听来自远方的TCP端口的连接请求
+ SYN-SENT：(Active; sent SYN. Waiting for a matching connection request after having sent a connection request.)再发送连接请求后等待匹配的连接请求
+ SYN-RECEIVED：(Sent and received SYN. Waiting for a confirming connection request acknowledgment after having both received and sent connection requests.)再收到和发送一个连接请求后等待对方对连接请求的确认
+ ESTABLISHED：(Connection established.)代表一个打开的连接
+ FIN-WAIT-1：(Closed; sent FIN.)等待远程TCP连接中断请求，或先前的连接中断请求的确认
+ FIN-WAIT-2：(Closed; FIN is acknowledged; awaiting FIN.)从远程TCP等待连接中断请求
+ CLOSE-WAIT：(Received FIN; waiting to receive CLOSE.)等待从本地用户发来的连接中断请求
+ CLOSING：(Closed; exchanged FIN; waiting for FIN.)等待远程TCP对连接中断的确认
+ LAST-ACK：(Received FIN and CLOSE; waiting for FIN ACK.)等待原来的发向远程TCP的连接中断请求的确认
+ TIME-WAIT：(In 2 MSL (twice the maximum segment length) quiet wait after close. )等待足够的时间以确保远程TCP接收到连接中断请求的确认
+ CLOSED：(Connection is closed.)没有任何连接状态



例如要查看当前Mysql默认端口80是否启动可以做如下操作


![](/images/Linux用netstat查看服务及监听端口详解/1654944231.880319.jpg)

netstat -ano来显示协议统计信息和TCP/IP网络连接

netstat -t/-u/-l/-r/-n【显示网络相关信息,-t:TCP协议,-u:UDP协议,-l:监听,-r:路由,-n:显示IP地址和端口号】

netstat -tlun【查看本机监听的端口】

netstat -an【查看本机所有的网络】

netstat -rn【查看本机路由表】

列出所有端口：netstat -a 

列出所有的TCP端口：netstat -at 

列出所有的UDP端口：netstat -au 

列出所有处于监听状态的socket：netstat -l 

列出所有监听TCP端口的socket：netstat -lt 

列出所有监听UDP端口的socket：netstat -lu 

找出程序运行的端口：netstat -ap | grep ssh 

找出运行在指定端口的进程：netstat -an | grep ‘:80’

几个有用查找：

```bash
1.查找请求数前20个IP（常用于查找攻来源）：
netstat -anlp|grep 80|grep tcp|awk '{print $5}'|awk -F: '{print $1}'|sort|uniq -c|sort -nr|head -n20
 
netstat -ant |awk '/:80/{split($5,ip,”:”);++A[ip[1]]}END{for(i in A) print A[i],i}' |sort -rn|head -n20
 
2.用tcpdump嗅探80端口的访问看看谁最高
tcpdump -i eth0 -tnn dst port 80 -c 1000 | awk -F”.” '{print $1″.”$2″.”$3″.”$4}' | sort | uniq -c | sort -nr |head -20
 
3.查找较多time_wait连接
netstat -n|grep TIME_WAIT|awk '{print $5}'|sort|uniq -c|sort -rn|head -n20
 
4.找查较多的SYN连接
netstat -an | grep SYN | awk '{print $5}' | awk -F: '{print $1}' | sort | uniq -c | sort -nr | more
 
5.根据端口列进程
netstat -ntlp | grep 80 | awk '{print $7}' | cut -d/ -f1
```

## 参考文章
[Linux用netstat查看服务及监听端口详解](https://blog.csdn.net/wade3015/article/details/90779669)

 
