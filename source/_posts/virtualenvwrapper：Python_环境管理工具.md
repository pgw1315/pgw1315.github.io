
---
title: virtualenvwrapper：Python_环境管理工具
date: 2022-06-08 12:18:21
author: Will
img: /images/banner/1_FjqLQ08MEk6jSKxpzjCcVw.jpeg
categories: Python
tags:
  - Python
  - 开发环境
---
        
## 概况

在使用 Python 开发的过程中，工程一多，难免会碰到不同的工程依赖不同版本的库的问题；亦或者是在开发过程中不想让物理环境里充斥各种各样的库，引发未来的依赖灾难。如果我们要同时开发多个应用程序，那这些应用程序都会共用一个 Python，就是安装在系统的 Python 3。如果应用 A 需要  jinja 2.7，而应用 B 需要 jinja 2.6 怎么办？此时，我们需要对于不同的工程使用不同的虚拟环境来保持开发环境以及宿主环境的清洁。为了不污染全局环境，通常都会使用环境隔离管理工具 `virtualenv` 与 `virtualenvwrapper`。
`virtualenv` 是在项目底下执行生成 `venv` 环境目录以此来进行管理，这非常适合使用诸如 `VSCode` 这种集成环境配置的开发工具；那么当通过 shell 来运行 `virtualenv` 时便会显得非常麻烦，因为每次 shell 关闭再打开后都需要重新配置环境参数。
`virtualenv` 需要每次使用 source 命令导入虚拟机运行环境，这一点非常麻烦，另外开发者还有可能忘记虚拟环境目录的建立位置，`virtualenvwrapper` 这一命令行工具就是通过对 `virtualenv` 进行封装，解决了上述问题。`virtualenvwrapper` 是将所有的 Python 项目虚拟环境环境都存放在一起，在使用 shell 配合小型开发工具就会非常方便。
## virtualenvwrapper 安装配置（Mac）


使用 pip3 安装，`virtualenv` 也会在此期间安装完毕

```bash
$ sudo pip3 install virtualenvwrapper
```


新建存放环境目录

```bash
$ mkdir -p ~/.virtualenvs
```


配置 `virtualenvwrapper` 环境（使用 python 3），打开 .zshrc，执行 `vim ~/.zshrc` 并写入(如果有安装`item2`与`oh-my-zsh`)

```bash
# 设置virtualenvwrapper    
export WORKON_HOME="~/.virtualenvs"    
export VIRTUALENVWRAPPER_PYTHON="/usr/local/bin/python3"    
# 打开终端自动启用    
source /usr/local/bin/virtualenvwrapper.sh
```


使配置生效

```bash
$ source ~/.zshrc
```

## virtualenvwrapper 使用


新建虚拟环境 `Test` 并指定 Python 版本为 Python3：

```bash
$ mkvirtualenv Test --python=python3
```

执行 `lsvirtualenv` 指令查看所有环境，环境 `Test` 位于 ~/Envs/test
```bash
$ lsvirtualenv    
Test    
==== 
```


在项目底下激活虚拟环境 `Test`：

```bash
$ workon Test
```


其他指令：

```bash
## 退出环境 Test    
deactivate    
## 删除环境 Test    
rmvirtualenv test    
## 更多指令可以在 shell 中输入 virtualenv 回车会有提示
```

## 在 VSCode 中使用虚拟环境


进入工作环境设置文件 `settings.json`，加入如下配置： 

```bash
"python.venvPath": "~/.virtualenvs"
```

具体操作界面如图所示：
![](/images/virtualenvwrapper：Python_环境管理工具/1654943943.082001.jpg)


重启 `VSCode` 即可看到配置已经成功被识别，便可以选择刚才新建的虚拟环境 `Test`：

![](/images/virtualenvwrapper：Python_环境管理工具/1654943943.202436.jpg)

## 参考文章
[virtualenvwrapper：Python_环境管理工具](https://zhuanlan.zhihu.com/p/70389886)