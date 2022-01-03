---
title: Linux中查看各文件夹大小命令du -h --max-depth=1
author: Will Holmes
categories: Linux
tags:
  - Linux
  - 文件管理
date: 2021-11-21 01:27:07
---

du [-abcDhHklmsSx] [-L <符号连接>][-X <文件>][--block-size][--exclude=<目录或文件>] [--max-depth=<目录层数>][--help][--version][目录或文件]
常用参数：
-a或-all 为每个指定文件显示磁盘使用情况，或者为目录中每个文件显示各自磁盘使用情况。
-b或-bytes 显示目录或文件大小时，以byte为单位。
-c或–total 除了显示目录或文件的大小外，同时也显示所有目录或文件的总和。
-D或–dereference-args 显示指定符号连接的源文件大小。
-h或–human-readable 以K，M，G为单位，提高信息的可读性。
-H或–si 与-h参数相同，但是K，M，G是以1000为换算单位,而不是以1024为换算单位。
-k或–kilobytes 以1024 bytes为单位。
-l或–count-links 重复计算硬件连接的文件。
-L<符号连接>或–dereference<符号连接> 显示选项中所指定符号连接的源文件大小。
-m或–megabytes 以1MB为单位。
-s或–summarize 仅显示总计，即当前目录的大小。
-S或–separate-dirs 显示每个目录的大小时，并不含其子目录的大小。
-x或–one-file-xystem 以一开始处理时的文件系统为准，若遇上其它不同的文件系统目录则略过。
-X<文件>或–exclude-from=<文件> 在<文件>指定目录或文件。
–exclude=<目录或文件> 略过指定的目录或文件。
–max-depth=<目录层数> 超过指定层数的目录后，予以忽略。
–help 显示帮助。
–version 显示版本信息。
  
**1> 要显示一个目录树及其每个子树的磁盘使用情况**
du /home/linux
这在/home/linux目录及其每个子目录中显示了磁盘块数。  
  
**2> 要通过以1024字节为单位显示一个目录树及其每个子树的磁盘使用情况**
du -k /home/linux  
这在/home/linux目录及其每个子目录中显示了 1024 字节磁盘块数。  
  
**3> 以MB为单位显示一个目录树及其每个子树的磁盘使用情况**
du -m /home/linux
这在/home/linux目录及其每个子目录中显示了 MB 磁盘块数。  
  
**4> 以GB为单位显示一个目录树及其每个子树的磁盘使用情况**
du -g /home/linux
这在/home/linux目录及其每个子目录中显示了 GB 磁盘块数。  
  
**5>查看当前目录下所有目录以及子目录的大小：**
du -h .
“.”代表当前目录下。也可以换成一个明确的路径
-h表示用K、M、G的人性化形式显示  
  
**6>查看当前目录下user目录的大小，并不想看其他目录以及其子目录：**
du -sh user
-s表示总结的意思，即只列出一个总结的值
du -h --max-depth=0 user
--max-depth=n表示只深入到第n层目录，此处设置为0，即表示不深入到子目录。  
  
**7>列出user目录及其子目录下所有目录和文件的大小：**
du -ah user
-a表示包括目录和文件  
  
**8>列出当前目录中的目录名不包括xyz字符串的目录的大小：**
du -h --exclude='*xyz*'  
  
**9>想在一个屏幕下列出更多的关于user目录及子目录大小的信息：**
du -0h user  
-0（杠零）表示每列出一个目录的信息，不换行，而是直接输出下一个目录的信息。  
  
**10>只显示一个目录树的全部磁盘使用情况**
du -s /home/linux
  
**11>查看各文件夹大小:du -h --max-depth=1**
 查看指定目录：
 代码如下：  其中 /path表示路径
   
```bash
du -h --max-depth=1 /path
```
 具体如下所示：
  
```bash
root@ubuntu4146:~# du -h --max-depth=1 /data/
1.1G	/data/gitlabDataa
8.0K	/data/test
241G	/data/gitlabData
809G	/data/home
15G	/data/OpenGrok
16K	/data/lost+found
1.1T	/data/
```
  
  
 我们发现  /data/home/ 目录占用最多，因此我们可以继续看那个目录占用的最多，如下所示：
```bash
root@ubuntu4146:/data/home# du -h --max-depth=1 /data/home/
141G	/data/home/wzm
62G	/data/home/lwc
421G	/data/home/hcy
16K	/data/home/zzp
16K	/data/home/zl
54G	/data/home/drj
122G	/data/home/sjq
4.1G	/data/home/ljs
6.7G	/data/home/ywm
809G	/data/home/
root@ubuntu4146:/data/home# 
```
  
 
  
  
