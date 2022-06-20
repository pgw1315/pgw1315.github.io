---
title: Git-版本发布
date: 2022-06-18 11:43:03
author: Will
img: /images/banner/git-command.png
categories: Git
tags:
  - Git
  
---
        
### 分支模型

![](/images/Git-版本发布/20210707101238633.png)

![](/images/Git-版本发布/20210707101247180.png)

### github release

查看releases，会有很多版本，这些版本其实就是Git中的标签。
Git标签可以将现有或过去的提交点（Commit Point）打上标签，这个标签
可能是项目的一个里程碑或者一个重要节点。

![](/images/Git-版本发布/20181213180038917.png)

创建Github一个版本发布：

![](/images/Git-版本发布/20181213180111885.png)
-a 证明这个标签是注解的
-m 标签消息

在本地打上标签后，还要推送到远程库中，在Git术语中称之为Sharing Tag
使用命令`git show v1.0.0`（v1.0.0是标签名），来查看某标签的信息

![](/images/Git-版本发布/20181213194224114.png)

确认无误后，可以将其通过`git push origin v1.0.0`命令推送到远程库，当然，较为罕见的情况，
你需要推送多个标签 `git push origin --tags`来代替


![git同步远程库标签](/images/Git-版本发布/20181213194503568.png)

### 删除标签并推送到远端



![](/images/Git-版本发布/201812132001379.png)


### 参考文章
[Git-版本发布](https://blog.csdn.net/qq_33745102/article/details/84992778)