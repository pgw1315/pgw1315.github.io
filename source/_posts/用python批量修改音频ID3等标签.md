---
title: Python3批量修改音频ID3等标签
author: Will Holmes
categories: Python
tags:
  - Python
  - eyeD3
  - Mp3
date: 2021-11-07 05:28:04
---

## eyeD3的安装
### 安装msgpack

不安装会报错distributed 1.21.8 requires msgpack, which is not installed
```bash 
pip3 install msgpack
```
2、安装magic，不安装的话，在import eyed3时会报错ImportError: failed to find libmagic. Check
your installation
```bash 
pip3 install python-magic-bin==0.4.14
```
3、安装eyeD3
```bash 
pip3 install eyeD3
```
## 使用eyeD3修改mp3标签

```python 
import eyed3  
audiofile = eyed3.load("hello word.mp3")  # 读取mp3文件
audiofile.initTag()  # 初始化所有标签信息，将之前所有的标签清除
audiofile.tag.artist = u"Jayson"  # 参与创作的艺术家
audiofile.tag.album = u"Love Visions"  # 唱片集
audiofile.tag.album_artist = u"art"  # 唱片艺术家
audiofile.tag.title = u"Hello World"  # 标题
audiofile.tag.track_num = 4  # 音轨编号，专辑内歌曲编号："#"
audiofile.tag.save() # 保存文件
```
