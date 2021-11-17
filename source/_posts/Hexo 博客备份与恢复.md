---
title: Hexo 博客备份与恢复 
author: Will Holmes
categories: Hexo
tags:
	- Hexo
	- 备份
date: 2021-11-15 12:55:46
---

# Hexo 博客备份与恢复

本文旨在解决在不同电脑上都能维护博客或配置、发布的内容丢失可恢复的问题。
观察部署到仓库的内容，我们可以看到上传的内容是 `public` 文件夹下的所有内容。事实上 `hexo-deploy-git` 插件是通过拷贝 `public` 文件夹内容到 `.deploy_git` 文件夹下，然后提交推送到远程分支上实现了网站文件的部署。
那我们的备份思路也可以这样，上传目录下的其他所有文件就可以了，同时我们不能修改博客的发布分支，因此考虑备份其他所有文件到新分支中。
最简单直接的方法就是在仓库创建一个新的分支，把本地所有的内容都上传到该分支上。但这样会备份一部分不必要的文件，例如 `public` 文件夹内容，它可以再次生成，就没有必要备份。
那具体要备份哪些文件呢？
## 备份
### 备份的文件列表
我们先看下，现在博客文件夹都有什么内容：


```
.
├──_config.yml
├── db.json
├── node_modules
├── package.json
├── package-lock.json
├── public
├── scaffolds
├── source
└── themes 
	├── next 
	└── landscape
```
这几个文件或文件夹的内容分别是：
1. `_config.yml` 文件：站点配置文件，很多功能、插件需要修改该配置文件应用生效。
2. `node_modules` 文件夹：包含依赖的模块。
3. `package.json` 文件：依赖的模块列表。说明见：[package.json:Specifics of npm’s package.json handling](https://www.npmjs.cn/files/package.json/)
4. `package-lock.json` 文件：依赖的模块安装记录。说明见：[npm-package-locks:An explanation of npm lockfiles](https://www.npmjs.cn/files/package-locks/)
5. `public` 文件夹：包含生成的网页静态文件。
6. `scaffolds` 文件夹：包含创建的文章、分类、标签界面的模板。博客的定制修改会对模板进行修改。
7. `source` 文件夹：包含生成网页所需要的源文件，包括包含我们心血的 Markdown 文稿，这也是最重要的内容。
8. `themes` 文件夹：其中 `landscape` 是默认的主题，其他文件夹是克隆下来时的主题。
我们可以参考 Hexo 初始化使用的仓库的备份列表，它的仓库是 [hexojs/hexo-starter](https://github.com/hexojs/hexo-starter)。我们看下它备份了哪些内容：


```
scaffolds
source
themes
.gitignore
.gitmodules
_config.yml
package.json
```

比对一下，它抛弃了：
1. `node_modules` & `package-lock.json`：这两部分内容，只要保留 `package.json`，执行 `npm install` 就可以下载、生成。
2. `public`：执行 `hexo g` 即可根据源文件生成网页内容。
这些可重新生成的文件都可以不上传，因为它们只要使用特定的命令操作即可恢复。
它增加了 `.gitmodules`，那它的作用又是什么呢？其实 `hexojs/hexo-starter` 是通过 Git 的 Submodule 功能来下载主题模块，本身仓库并不备份主题文件。考虑下我们需要如何备份主题文件目录，有两个方案：
1. 一个方案是将其内容全部上传进行备份，这样可以保证原主题的更新不会影响你原先配置的效果。
2. 另一个方案是像 `hexo-starter` 仓库一样通过 Git 的 Submodule 功能来管理，这样可以进行主题的更新。
这里我选择和 Hexo 初始化仓库一样使用 Git 子模块的方式进行主题的备份处理。不过通过子模块管理的方式我们恢复时仅会同步到 Next 主题的原文件，没法直接同步我们对主题配置文件或其他文件的修改，因为我们没有权限提交修改到 Next 仓库中。因此我创建了了个 `themes_custom` 文件夹来存放对应主题的修改的备份，这样子我们同步之后，只需要对比一下内容手动把这些配置应用过去就可以快速完成对主题的配置。
最终备份文件列表如下：

```
scaffolds
source
themes
themes_custom/next
.gitignore
.gitmodules
_config.yml
package.json
```
### 具体操作
有了方案之后，我们备份的具体操作如下：
1. 先修改 `.gitignore` 文件，查看之后由于原文件已经忽略了 `public` 和 `node_modules` 文件夹，因此仅需要添加 `package-lock.json` 到忽略清单中。
2. 我们可以删除不使用的主题 `landscape` 或者把主题路径添加到忽略列表中。
3. 创建 `themes_custom/next` 文件夹，将对主题进行的配件修改的文件拷贝一份到这里
4. 执行以下命令，在本地创建备份仓库：

```bash
$ cd blog
$ git init
#已初始化空的 Git 仓库于 blog/.git/
$ git submodule add https://github.com/theme-next/hexo-theme-next.git themes/next
#添加位于 'themes/next' 的现存仓库到索引
$ git add .$ git commit -m "init blog backup"
```

1. 将备份内容 push 到远程仓库的备份分支 `hexo` 上：

```bash
$ git branch -m master hexo
$ git remote add origin https://github.com/Mupceet/mupceet.github.io.git
$ git push -u origin hexo:hexo
```
 这里，我对分支进行了重命名以减少记忆负担
经过以上步骤，我们就备份了所有必要的文件。后续更新博客也要及时地将这些文件进行提交并上传完成备份。
## 恢复
有了备份之后，在另一台电脑上创建博客，或者是恢复备份时，就可以直接使用我们备份的内容进行操作。
1. 环境准备
 具体见 [Hexo 博客搭建与主题配置（零基础版）](https://mupceet.com/2019/08/build-blog-based-on-hexo/)一文。
2. 克隆备份的内容

```bash
$ git clone --recursive -b hexo https://github.com/Mupceet/mupceet.github.io.git blog
```
1. 下载 npm 依赖模块

```bash
$ cd blog$ npm install
```

1. 恢复主题配置
 将 `themes_custom` 文件夹中对主题的配置的修改恢复到对应的主题文件夹中，这里建议使用对比的方式对其进行修改，而不是直接覆盖，这样就完成了主题的配置。
5. 克隆原博客内容

```bash
$ cd blog$ git clone https://github.com/Mupceet/mupceet.github.io.git .deploy\_git
```

1. 正常更新博客

```bash
$ hexo g
$ hexo s
$ hexo d
```
可以看到，使用备份文件恢复博客的环境是非常简单的，强烈建议大家搭建好博客之后增加一个备份操作。