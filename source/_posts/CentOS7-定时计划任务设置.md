---
title: CentOS7 定时计划任务设置
author: Will Holmes
categories: Linux
tags:
  - CentOS7
  - 定时任务
  - Crontabs
date: 2021-11-07 04:38:29
---


## 概述

就像再windows上有计划任务一样，centos7 自然也有计划任务，而且设置更为灵活，好用。再centos7 上可以利用crontab 来执行计划任务，
依赖与 crond 的系统服务，这个服务是系统自带的，可以直接查看状态，启动，停止。  

## 安装 crontabs服务并设置开机自启
```bash 
yum install crontabs  
systemctl enable crond （设为开机启动）  
systemctl start crond（启动crond服务）  
systemctl status crond （查看状态） 
```


## 设置用户自定义定时任务
```bash 
vi /etc/crontab
```
 
可以看到： 

```bash 
Example of job definition:  
.---------------- minute (0 - 59)  
| .------------- hour (0 - 23)  
| | .---------- day of month (1 - 31)  
| | | .------- month (1 - 12) OR jan,feb,mar,apr ...  
| | | | .---- day of week (0 - 6) (Sunday=0 or 7) OR
sun,mon,tue,wed,thu,fri,sat  
| | | | |  
* * * * * user-name command to be executed  


即：  
分钟(0-59) 小时(0-23) 日(1-31) 月(11-12) 星期(0-6,0表示周日) 用户名 要执行的命令

  * */30 * * * root /usr/local/mycommand.sh (每天，每30分钟执行一次 mycommand命令)

  * * 3 * * * root /usr/local/mycommand.sh (每天凌晨三点，执行命令脚本，PS:这里由于第一个的分钟没有设置，那么就会每天凌晨3点的每分钟都执行一次命令)

  * 0 3 * * * root /usr/local/mycommand.sh (这样就是每天凌晨三点整执行一次命令脚本)

  * */10 11-13 * * * root /usr/local/mycommand.sh (每天11点到13点之间，每10分钟执行一次命令脚本，这一种用法也很常用)

  * 10-30 * * * * root /usr/local/mycommand.sh (每小时的10-30分钟，每分钟执行一次命令脚本，共执行20次)

  * 10,30 * * * * * root /usr/local/mycommand.sh (每小时的10,30分钟，分别执行一次命令脚本，共执行2次）
```
### 保存生效

加载任务,使之生效：`crontab /etc/crontab`

查看任务：`crontab -l`  
$ crontab -u 用户名 -l （列出用户的定时任务列表）

PS：特别注意，crond的任务计划，
有并不会调用用户设置的环境变量，它有自己的环境变量，当你用到一些命令时，比如mysqldump等需要环境变量的命令，手工执行脚本时是正常的，但用crond执行的时候就会不行，这时你要么写完整的绝对路径，要么将环境变量添加到
/etc/crontab 中。

好了，计划任务就是这么简单了，但是计划任务，执行的语句如果是多条，则需要用药shell脚本，自己先写一个shell脚本，然后在计划任务中，执行这个脚本即可。至于shell脚本的写法，
这里不赘述。

