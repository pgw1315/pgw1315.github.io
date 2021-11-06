---
title: Python判断当前操作系统类型以及os/sys/platform模块简介
author: Will Holmes
categories: Python
tags:
  - Python
  - platform

date: 2021-11-07 04:44:00
---


## 判断操作系统类型

```python

    #coding=utf-8
    
    
    import platform
    
    def TestPlatform( ):
        print ("----------Operation System--------------------------")
        ##  获取Python版本
        print platform.python_version()
    
        ##   获取操作系统可执行程序的结构，，(’32bit’, ‘WindowsPE’)
        print platform.architecture()
    
        ##   计算机的网络名称，’acer-PC’
        print platform.node()
    
        #获取操作系统名称及版本号，’Windows-7-6.1.7601-SP1′
        print platform.platform()  
    
        #计算机处理器信息，’Intel64 Family 6 Model 42 Stepping 7, GenuineIntel’
        print platform.processor()
    
        ## 获取操作系统中Python的构建日期
        print platform.python_build()
    
        ##  获取系统中python解释器的信息
        print platform.python_compiler()
    
        if platform.python_branch()=="":
            print platform.python_implementation()
            print platform.python_revision()
        print platform.release()
        print platform.system()
    
        #print platform.system_alias()
        ##  获取操作系统的版本
        print platform.version()
    
        ##  包含上面所有的信息汇总
        print platform.uname()
    
    def UsePlatform( ):
        sysstr = platform.system()
        if(sysstr =="Windows"):
            print ("Call Windows tasks")
        elif(sysstr == "Linux"):
            print ("Call Linux tasks")
        else:
            print ("Other System tasks")
    
    
    
    
    if __name__ == "__main__" :
    
        TestPlatform( )
    
        UsePlatform( )
    

```
## Python与操作系统有关的模块

* * *

### Os模块

* * *

Python的标准库中的os模块主要涉及普遍的操作系统功能。可以在Linux和Windows下运行，与平台无关。  
os.sep 可以取代操作系统特定的路径分割符。  
os.name字符串指示你正在使用的平台。比如对于Windows，它是’nt’，而对于Linux/Unix用户，它是’posix’。  
os.getcwd()函数得到当前工作目录，即当前Python脚本工作的目录路径。  
os.getenv()和os.putenv()函数分别用来读取和设置环境变量。  
os.listdir()返回指定目录下的所有文件和目录名。  
os.remove()函数用来删除一个文件。  
os.system()函数用来运行shell命令。  
os.linesep字符串给出当前平台使用的行终止符。例如，Windows使用’\r\n’，Linux使用’\n’而Mac使用’\r’。  
os.path.split()函数返回一个路径的目录名和文件名。  
os.path.isfile()和os.path.isdir()函数分别检验给出的路径是一个文件还是目录。  
os.path.existe()函数用来检验给出的路径是否真地存在  
os和os.path模块  
os.listdir(dirname)：列出dirname下的目录和文件  
os.getcwd()：获得当前工作目录  
os.curdir:返回但前目录（’.’)  
os.chdir(dirname):改变工作目录到dirname  
os.path.isdir(name):判断name是不是一个目录，name不是目录就返回false  
os.path.isfile(name):判断name是不是一个文件，不存在name也返回false  
os.path.exists(name):判断是否存在文件或目录name  
os.path.getsize(name):获得文件大小，如果name是目录返回0L  
os.path.abspath(name):获得绝对路径  
os.path.normpath(path):规范path字符串形式  
os.path.split(name):分割文件名与目录（事实上，如果你完全使用目录，它也会将最后一个目录作为文件名而分离，同时它不会判断文件或目录是否存在）  
os.path.splitext():分离文件名与扩展名  
os.path.join(path,name):连接目录与文件名或目录  
os.path.basename(path):返回文件名  
os.path.dirname(path):返回文件路径

### Sys模块

* * *

sys.argv: 实现从程序外部向程序传递参数。  
sys.exit([arg]): 程序中间的退出，arg=0为正常退出。  
sys.getdefaultencoding(): 获取系统当前编码，一般默认为ascii。  
sys.setdefaultencoding():
设置系统默认编码，执行dir（sys）时不会看到这个方法，在解释器中执行不通过，可以先执行reload(sys)，在执行
setdefaultencoding(‘utf8’)，此时将系统默认编码设置为utf8。（见设置系统默认编码 ）  
sys.getfilesystemencoding(): 获取文件系统使用编码方式，Windows下返回’mbcs’，mac下返回’utf-8’.  
sys.path: 获取指定模块搜索路径的字符串集合，可以将写好的模块放在得到的某个路径下，就可以在程序中import时正确找到。  
sys.platform: 获取当前系统平台。  
sys.stdin,sys.stdout,sys.stderr stdin , stdout , 以及stderr 变量包含与标准I/O 流对应的流对象.
如果需要更好地控制输出,而print 不能满足你的要求, 它们就是你所需要的. 你也可以替换它们, 这时候你就可以重定向输出和输入到其它设备( device
), 或者以非标准的方式处理它们

### Paltform模块

* * *

platform.system() 获取操作系统类型，windows、linux等  
platform.platform() 获取操作系统，Darwin-9.8.0-i386-32bit  
platform.version() 获取系统版本信息 6.2.0  
platform.mac_ver()  
platform.win32_ver() (‘post2008Server’, ‘6.2.9200’, ”, u’Multiprocessor Free’)

