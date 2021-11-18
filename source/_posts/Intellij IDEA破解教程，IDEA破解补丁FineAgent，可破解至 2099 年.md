---
title: Intellij IDEA破解补丁FineAgent，可破解至2099
author: Will Holmes
categories: 工具
tags:
 - IDEA
 - 破解
 - 教程
date: 2021-11-18 09:33:41
---

有网友问我，IDEA 产品能否永久激活呢？目前，我在网上逛了一圈，又发现某大佬开发了一款新的破解补丁，叫**FineAgent**，如下图所示：
![IDEA破解成功](http://img.javatiku.cn/20210812172447.png "IDEA破解成功")

# 说明
* 本教程适用于**JetBrains 全系列产品，可放心使用；
* 本教程适用于 **Windows**/**Mac**/**Linux** 系统；
* 本教程适用于 **JetBrains 全系列产品**，包括 **IntelliJ IDEA**、**APPCode**、**CLion**、**DataGrip**、**GoLand**、**PhpStorm**、**PyCharm**、**Rider**、**RubyMine** 和 **WebStorm**。

# 教程演示
下面的教程演示使用的是 **Windows** 系统，安装的 **IDEA 版本为 2021.2**。

### 1、下载自己需要的IDEA
IDEA 老版本下载地址：<https://www.jetbrains.com/zh-cn/idea/download/other.html>

### 2、安装IDEA
这个操作就easy了，我们按照常规的安装方式安装就行，我就不详细说了。
如果你真的不会的话，那说明你确实不太适合做程序员~

### 3、重置IDEA并点击试用
1）如果你的IDEA是新安装的，可以直接点击 **试用**，跳过这一步接下来的操作。
![IDEA试用](http://img.javatiku.cn/20210424231958.png "IDEA试用")
2）如果你的电脑之前安装过IDEA且超过试用期（还在试用期的可以忽略该步骤），可以直接通过我这里提供的重置脚本进行重置就行，其原理是删除jetbrains全系列产品软件试用相关目录（其他配置不受影响）。
![IDEA重置脚本](http://img.javatiku.cn/20210423185059.png "IDEA重置脚本")
IDEA重置脚本安全可靠，大家可以看看下面的源码哈：
![IDEA重置脚本](http://img.javatiku.cn/20210423185142.png "IDEA重置脚本")
重置IDEA之后，打开IDEA，点击试用，按照图示选择试用。
![IDEA试用](http://img.javatiku.cn/20210424231958.png "IDEA试用")
**说明：最新的 IDEA 2021.2.3 版本 IDEA 界面发生了变化，取消了直接试用 30 天的按钮，需要我们先注册一个 JetBrains（这里我用的 GitHub 账号注册的）**，如下:
![IDEA试用](http://www.javatiku.cn/usr/img/4816dd4fc758afa9e0e4e717059ed162.png "IDEA试用")
注册账号并登录后，就可以试用 IDEA 了：
![IDEA试用](http://www.javatiku.cn/usr/img/96f2e7b62949112c63fe5eb498bd2d64.png "IDEA试用")

### 4、下载破解补丁
该破解方法需要用到的文件都准备好了，通过下方的网盘链接下载就行。


###### [蓝奏云下载](https://wwe.lanzoui.com/iWZ0Nwm96od)

压缩包目录结构：
```
FineAgent.zip
|---激活码
|---FineAgent.jar
reset_script.zip
```


### 5、安装破解补丁
如果你之前安装过 IDEA, 那么修改过的 hosts 文件要还原回去、引用过的补丁要移除掉等, 不然可能会有各种奇奇怪怪的问题。
打开IDEA，你可以在Help -> Eidt Custom VM Options... ，参考如下图所示:
![安装破解补丁](http://img.javatiku.cn/20210812172939.png "安装破解补丁")
由于我把压缩包内的`FineAgent.jar`直接放在D盘的根目录，所以一定要先清除vmoptions文件内其它的`-javaagent:xxx`配置（否则会存在IDEA打不开的情况），然后再在最后一行加上如下代码：
```
-javaagent:d:/FineAgent.jar
```
![IDEA破解教程](http://img.javatiku.cn/20210812173210.png "IDEA破解教程")

### 6、重启IDEA
安装完成后，重启 idea。

### 7、通过指定的注册码来激活IDEA
重启后，便会提示你输入激活码，激活码在压缩包内 `ActivationCode.txt` ，这时，你可以用压缩包内的激活码进行激活，如下图
![IDEA激活码](http://img.javatiku.cn/20210812173504.png "IDEA激活码")
此时，点击Activate，便激活成功了。

### 8、如何验证是否激活成功呢？
你直接进入 IDEA 主界面，在上方的菜单栏点击 `Help` -> `Register` 查看当前激活状态。
![IDEA破解教程](http://img.javatiku.cn/20210812173753.png "IDEA破解教程")
这时候你就可以看到，已经激活至 2099 年啦，爽歪歪~
![IDEA破解教程](http://img.javatiku.cn/20210812173856.png "IDEA破解教程")

### 为何IDEA打不开？
一个最主要的原因是：你在`vmoptions`文件配置的路径不对。
如果你想打开IDEA，那么需要找到以`vmoptions`后缀结尾的文件(一般位于`C:\Users\用户名\AppData\Roaming\JetBrains\IntelliJIdea2021.2`目录下)，找到 `idea64.exe.vmoptions`，打开之后，删除 -javaagent:xxx的信息，即可打开，虽然IDEA能打开了，但是很不幸你未破解成功！
要想重新破解，那么请重复上述步骤！！！
希望本教程对你有所帮助！


### 引用
[http://www.javatiku.cn/idea/5.html](http://www.javatiku.cn/idea/5.html)