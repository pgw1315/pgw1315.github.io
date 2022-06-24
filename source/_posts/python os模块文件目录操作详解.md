---
title: python os模块文件目录操作详解
date: 2022-06-22 10:25:50
author: Will
img: /images/banner/filemanager.jpeg
categories: Python
tags:
  - 文件管理
---


###  1. os函数列表

```python
os.mknod("text.txt")：创建空文件
os.system():运行shell命令
os.exit():终止当前进程
os.getcwd() 取得当前工作目录
os.getenv()和os.putenv:分别用来读取和设置环境变量
os.environ['MY_USER']
os.chdir('dirname') 改变目录
os.mkdir/makedirs('dirname')创建目录/多层目录
os.rmdir/removedirs('dirname') 删除目录/多层目录
os.remove(‘path/filename’) 删除文件
os.rename(oldname, newname) 重命名文件
os.walk() 生成目录树下的所有文件名
os.stat（file）:获得文件属性
os.listdir('dirname') 列出指定目录的文件
os.chmod() 改变目录权限
os.path.abspath(path) #返回绝对路径
os.path.basename(path) #返回文件名
os.path.commonprefix(list) #返回list(多个路径)中，所有path共有的最长的路径。'
os.path.dirname(path) #返回文件路径
os.path.exists(path) #路径存在则返回True,路径损坏返回False
os.path.lexists              #路径存在则返回True,路径损坏也返回True
os.path.expanduser(path)    #把path中包含的"~"和"~user"转换成用户目录
os.path.expandvars(path)    #根据环境变量的值替换path中包含的”$name”和”${name}”
os.path.getatime(path)      #返回最后一次进入此path的时间。
os.path.getmtime(path)      #返回在此path下最后一次修改的时间。
os.path.getctime(path)      #返回path的大小
os.path.getsize(path)       #返回文件大小，如果文件不存在就返回错误
os.path.isabs(path)         #判断是否为绝对路径
os.path.isfile(path)        #判断路径是否为文件
os.path.isdir(path)         #判断路径是否为目录
os.path.islink(path)        #判断路径是否为链接
os.path.ismount(path)       #判断路径是否为挂载点（）
os.path.join(path1[, path2[, ...]])         #把目录和文件名合成一个路径'
os.path.normcase(path)      #转换path的大小写和斜杠
os.path.normpath(path)      #规范path字符串形式
os.path.realpath(path)      #返回path的真实路径
os.path.relpath(path[, start])  #从start开始计算相对路径'
os.path.samefile(path1, path2)  #判断目录或文件是否相同'
os.path.sameopenfile(fp1, fp2)  #判断fp1和fp2是否指向同一文件'
os.path.samestat(stat1, stat2)  #判断stat tuple stat1和stat2是否指向同一个文件'
os.path.split(path)     #把路径分割成dirname和basename，返回一个元组
os.path.splitdrive(path)     #一般用在windows下，返回驱动器名和路径组成的元组
os.path.splitext(path)  #分割路径，返回路径名和文件扩展名的元组
os.path.splitunc(path)      #把路径分割为加载点与文件
os.path.walk(path, visit, arg)  #遍历path，进入每个目录都调用visit函数，visit函数必须有'
#3个参数(arg, dirname, names)，dirname表示当前目录的目录名，names代表当前目录下的所有
#文件名，args则为walk的第三个参数
os.path.supports_unicode_filenames      #设置是否支持unicode路径名
os.sep:取代操作系统特定的路径分隔符
os.name:指示你正在使用的工作平台。比如对于Windows，它是'nt'，而对于Linux/Unix用户，它是'posix'。

```

###  2. os.stat方法

```python
>>> import os
>>> print os.stat("/root/python/zip.py")
(33188, 2033080, 26626L, 1, 0, 0, 864, 1297653596, 1275528102, 1292892895)
>>> print os.stat("/root/python/zip.py").st_mode   #权限模式
33188
>>> print os.stat("/root/python/zip.py").st_ino   #inode number
2033080
>>> print os.stat("/root/python/zip.py").st_dev    #device
26626
>>> print os.stat("/root/python/zip.py").st_nlink  #number of hard links
1
>>> print os.stat("/root/python/zip.py").st_uid    #所有用户的user id
0
>>> print os.stat("/root/python/zip.py").st_gid    #所有用户的group id
0
>>> print os.stat("/root/python/zip.py").st_size  #文件的大小，以位为单位
864
>>> print os.stat("/root/python/zip.py").st_atime  #文件最后访问时间
1297653596
>>> print os.stat("/root/python/zip.py").st_mtime  #文件最后修改时间
1275528102
>>> print os.stat("/root/python/zip.py").st_ctime  #文件创建时间
1292892895
fp = open("text.txt",w):直接打开一个文件，如果文件不存在就创建文件

```

###  3. 关于open的模式

```python
w 写方式
a 追加模式打开（从EOF开始，必要时创建新文件）
r+ 以读写模式打开
w+ 以读写模式打开
a+ 以读写模式打开
rb 以二进制读模式打开
wb 以二进制写模式打开 (参见 w )
ab 以二进制追加模式打开 (参见 a )
rb+ 以二进制读写模式打开 (参见 r+ )
wb+ 以二进制读写模式打开 (参见 w+ )
ab+ 以二进制读写模式打开 (参见 a+ )

```

###  4. 关于文件的函数

```python
fp.read([size])   

```

size为读取的长度，以byte为单位

```python
fp.readline([size])  

```

读一行，如果定义了size，有可能返回的只是一行的一部分

```python
fp.readlines([size]) 

```

把文件每一行作为一个list的一个成员，并返回这个list。其实它的内部是通过循环调用readline()来实现的。如果提供size参数，size是表示读取内容的总长，也就是说可能只读到文件的一部分。

```python
fp.write(str)  

```

把str写到文件中，write()并不会在str后加上一个换行符

```python
fp.writelines(seq)  

```

把seq的内容全部写到文件中(多行一次性写入)。这个函数也只是忠实地写入，不会在每行后面加上任何东西。

```python
fp.close()  

```

关闭文件。python会在一个文件不用后自动关闭文件，不过这一功能没有保证，最好还是养成自己关闭的习惯。 如果一个文件在关闭后还对其进行操作会产生ValueError

```python
fp.flush()     

```

把缓冲区的内容写入硬盘

```python
fp.fileno()    

```

返回一个长整型的”文件标签“

```python
fp.isatty()      

```

文件是否是一个终端设备文件（unix系统中的）

```python
fp.tell()     

```

返回文件操作标记的当前位置，以文件的开头为原点

```python
fp.next()  

```

返回下一行，并将文件操作标记位移到下一行。把一个file用于for … in file这样的语句时，就是调用next()函数来实现遍历的。

```python
fp.seek(offset[,whence])  

```

将文件打操作标记移到offset的位置。这个offset一般是相对于文件的开头来计算的，一般为正数。但如果提供了whence参数就不一定了，whence可以为0表示从头开始计算，1表示以当前位置为原点计算。2表示以文件末尾为原点进行计算。需要注意，如果文件以a或a+的模式打开，每次进行写操作时，文件操作标记会自动返回到文件末尾。

```python
fp.truncate([size])      

```

把文件裁成规定的大小，默认的是裁到当前文件操作标记的位置。如果size比文件的大小还要大，依据系统的不同可能是不改变文件，也可能是用0把文件补到相应的大小，也可能是以一些随机的内容加上去。

###  5 实战

#### 5.1 创建目录

```python
os.mkdir("file")

```

#### 5.2 复制文件:

```python
shutil.copyfile("oldfile","newfile")    

```

oldfile和newfile都只能是文件

```python
shutil.copy("oldfile","newfile")   

```

oldfile只能是文件夹，newfile可以是文件，也可以是目标目录

```python
shutil.copytree("olddir","newdir")    

```

复制文件夹.olddir和newdir都只能是目录，且newdir必须不存在

#### 5.3 重命名文件（目录）

```python
os.rename("oldname","newname")  

```

文件或目录都是使用这条命令

#### 5.4 移动文件（目录）

```python
shutil.move("oldpos","newpos") 

```

#### 5.5 删除

只能删除空目录

```python
os.rmdir("dir")

```

空目录、有内容的目录都可以删

```python
shutil.rmtree("dir")  

```

#### 5.6 转换目录，换路径

```python
os.chdir("path")   

```

```python
$ cat os1.py 
#!/usr/bin/python
#---coding: utf-8---
import os

print("环境变量 SHLVL:", os.getenv("SHLVL"))
print("当前目录:", os.getcwd())
dir = os.getcwd()

print("当前目录下内容：", os.listdir(dir))
print("当前目录下内容：", os.system("ls"))

print("创建目录hello", os.mkdir('hello'))
print("删除目录hello", os.rmdir('hello'))

print("创建目录foo", os.mkdir(dir + '/foo'))
print("重命名foo为bar", os.rename('foo','bar'))

print("创建文件test.txt", os.mknod('test.txt'))

```

```bash
$ python3.8 os1.py 
环境变量 SHLVL: 1
当前目录: /root/python/os
当前目录下内容： ['os2.py', 'os1.py']
os1.py	os2.py
当前目录下内容： 0
创建目录hello None
删除目录hello None
创建目录foo None
重命名foo为bar None
创建文件test.txt None

```

#### 5.7 获取env环境变量

在python代码中，可以读取env变量，如：

```python
import os
username = os.environ['MY_USER']
password = os.environ['MY_PASS']
print("Running with user: %s" % username)

```

比如运行在容器的python脚本需要捕捉环境变量env。docker run可以动态根据自己的需求修改环境变量。

```python
docker run -e MY_USER=test -e MY_PASS=12345 ... <image-name> ...

```

#### 5.8 批量创建指定名称的文件夹

```python
import os, sys
def MkDir():
    path = './file/'#创建文件路径
    i = 0
    for i in range(200): #创建文件个数
        file_name = path + str(i)
        os.mkdir(file_name)
        i=i+1
        file_name_child = file_name + "/left_colorimages"  
        os.mkdir(file_name_child)
MkDir()

```

#### 5.9 判断目录是否存在

```python
import os
dirs = '/Users/joseph/work/python/'

if not os.path.exists(dirs):
    os.makedirs(dirs)

```

#### 5.10 判断文件是否存在

```python
import os
filename = '/Users/joseph/work/python/poem.txt'

if not os.path.exists(filename):
    os.system(r"touch {}".format(path))#调用系统命令行来创建文件

```

![在这里插入图片描述](/images/python os模块文件目录操作详解/bb6a726ffd424868be6ba585ef8203a0.gif)


###  参考文章
[python os模块文件目录操作详解](https://blog.csdn.net/xixihahalelehehe/article/details/104253123)