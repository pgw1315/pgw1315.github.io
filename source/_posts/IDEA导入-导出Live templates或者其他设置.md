---
title: IDEA导入/导出Live templates或者其他设置
author: Will Holmes
categories: 工具
tags:
  - java
  - Tools
  - IDEA
date: 2021-11-07 04:21:42
---


# IDEA导入/导出live templates或者其他设置

### 导出

  1. 在菜单栏选择 `File` | `Manage IDE Settings` | `Export Settings`

  2. 在打开的导出弹窗中，选择需要导出的项目，如果我们只需要导出 **Live templates** ，那就只选择 **Live templates** 即可，然后选择一个需要导出的位置并设置一个存储的文件名（默认是settings.zip）

  3. 点击 **OK** 进行导出，导出的文件可以导入到其他IDEA中进行使用

### 导入

  1. 在菜单栏选择 `File` | `Manage IDE Settings` | `Import Settings`

  2. 选择之前导出的配置文件，点击 **OK**

  3. 在弹出的窗口中选择需要导入的项目，例如 **Live templates** 然后点击 **OK**

  4. 重启IDEA使新配置生效

### 参考

> [Share live templates](https://www.jetbrains.com/help/idea/sharing-live-
> templates.html#export)

