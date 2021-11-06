---
title: Python遍历文件夹的两种方法
author: Will Holmes
categories: Python
tags:
  - Python
  - 文件管理
date: 2021-11-07 03:25:32
---


在处理数据的过程中，经常需要遍历文件夹，如果远程服务器的文件是分布式存储，遍历需要更快的速度。

  1. 一种是通过os.walk()遍历，直接处理文件即可。
  2. 一种是通过pathlib.Path().rglob()遍历，需要过滤出文件，速度较快。注意glob()不支持递归遍历。

实测pathlib.Path().rglob()方案要快于os.walk()方案。

os.walk的遍历方式，支持筛选后缀和排序：

```python

    def traverse_dir_files(root_dir, ext=None, is_sorted=True):
        """
        列出文件夹中的文件, 深度遍历
        :param root_dir: 根目录
        :param ext: 后缀名
        :param is_sorted: 是否排序，耗时较长
        :return: [文件路径列表, 文件名称列表]
        """
        names_list = []
        paths_list = []
        for parent, _, fileNames in os.walk(root_dir):
            for name in fileNames:
                if name.startswith('.'):  # 去除隐藏文件
                    continue
                if ext:  # 根据后缀名搜索
                    if name.endswith(tuple(ext)):
                        names_list.append(name)
                        paths_list.append(os.path.join(parent, name))
                else:
                    names_list.append(name)
                    paths_list.append(os.path.join(parent, name))
        if not names_list:  # 文件夹为空
            return paths_list, names_list
        if is_sorted:
            paths_list, names_list = sort_two_list(paths_list, names_list)
        return paths_list, names_list
    

```
pathlib.Path().rglob()的遍历方式，也支持筛选后缀和排序：

  * 注意：通过判断是否有后缀名（"."），来判断是文件还是文件夹
  * list()之后再for循环，避免出现Bug

```python

    def traverse_dir_files(root_dir, ext=None, is_sorted=True):
        """
        列出文件夹中的文件, 深度遍历
        :param root_dir: 根目录
        :param ext: 后缀名
        :param is_sorted: 是否排序，耗时较长
        :return: [文件路径列表, 文件名称列表]
        """
        names_list = []
        paths_list = []
        for path in list(pathlib.Path(root_dir).rglob("*")):
            path = str(path)
            name = path.split("/")[-1]
            if name.startswith('.') or "." not in name:  # 去除隐藏文件
                continue
            if ext:  # 根据后缀名搜索
                if name.endswith(ext):
                    names_list.append(name)
                    paths_list.append(path)
            else:
                names_list.append(name)
                paths_list.append(path)
        if not names_list:  # 文件夹为空
            return paths_list, names_list
        if is_sorted:
            paths_list, names_list = sort_two_list(paths_list, names_list)
        return paths_list, names_list
    

```
参考：

  * [How to use glob() to find files recursively?](https://stackoverflow.com/questions/2186525/how-to-use-glob-to-find-files-recursively)

