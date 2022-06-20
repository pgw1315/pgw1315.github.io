---
title: VScode中使用git合并分支
date: 2022-06-20 11:58:18
author: Will
img: /images/banner/vscode.png
categories: 工具
tags:
  - VScode
---


### 前提

首先我们有两个分支，一个是test分支，一个是main分支。我们现在要把test分支合并到main分支里面去。（就是说，test分支是比main分支早一个版本的）
另外，**我们默认本地分支和线上分支版本是一样的**，如果不一样要先pull一下。

test分支的内容 
![](/images/vscode中使用git合并分支/20210719182820105.png)
main分支的内容 
![](/images/vscode中使用git合并分支/20210719182844500.png)


### 1.把分支切换到本地的main分支

![](/images/vscode中使用git合并分支/20210719182939631.png)
注意，本地的main分支是没有origin开头的。

### 2. 点击合并分支

![](/images/vscode中使用git合并分支/20210719183028248.png)

### 3. 选择开发的分支

![](/images/vscode中使用git合并分支/20210719183138645.png)
这里需要选择分支，我们选择本地的test分支。

![](/images/vscode中使用git合并分支/20210719183234835.png)
然后，我们看到main分支里面，就会出现刚刚test的内容了。而这时，只是本地的main分支合并了，线上的main分支还没被合并。我们需要再推送一下。

### 4. 将main分支推送到线上

![](/images/vscode中使用git合并分支/20210719183358597.png)

### 5.合并完成



### 参考文章
[vscode中使用git合并分支](https://blog.csdn.net/weixin_44004835/article/details/118908695)