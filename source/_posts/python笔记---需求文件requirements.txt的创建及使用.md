---
title: python需求文件requirements.txt的创建及使用
date: 2022-06-26 08:19:44
author: Will
img: /images/banner/Python-Learning.webp
categories: Python
tags:
  - Python
---


python项目中必须包含一个 requirements.txt 文件，用于记录所有依赖包及其精确的版本号。以便新环境部署。

## 在虚拟环境中使用pip生成：

```bash
pip freeze >requirements.txt
```
安装或升级包后，最好更新这个文件。

需求文件的内容示例如下：

```python
alembic==0.8.6
bleach==1.4.3
click==6.6
dominate==2.2.1
Flask==0.11.1
Flask-Bootstrap==3.3.6.0
Flask-Login==0.3.2
Flask-Migrate==1.8.1
Flask-Moment==0.5.1
Flask-PageDown==0.2.1
Flask-Script==2.0.5
Flask-SQLAlchemy==2.1
Flask-WTF==0.12
html5lib==0.9999999
itsdangerous==0.24
Jinja2==2.8
Mako==1.0.4
Markdown==2.6.6
MarkupSafe==0.23
PyMySQL==0.7.5
python-editor==1.0.1
six==1.10.0
SQLAlchemy==1.0.14
visitor==0.1.3
Werkzeug==0.11.10
WTForms==2.1

```

当需要创建这个虚拟环境的完全副本，可以创建一个新的虚拟环境，并在其上运行以下命令：

```bash
pip install -r requirements.txt
```



## 参考文章
[python笔记---需求文件requirements.txt的创建及使用](https://blog.csdn.net/loyachen/article/details/52028825)