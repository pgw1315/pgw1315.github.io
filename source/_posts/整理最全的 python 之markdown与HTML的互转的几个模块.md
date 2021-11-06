---
title: 整理最全的 python 之markdown与HTML的互转的几个模块
author: Will Holmes
categories: Python
tags:
  - Python
  - MarkDown
date: 2021-11-07 04:07:17
---


# 一、说明：

今天突然想着学习一下如何将markdown和HTML互转的知识，因为我在CSDN的写的博客可以导出的时候有俩种方式，所以想着也可以把他们相互转化下。我觉得python现在很成熟了，肯定有这方面的轮子。于是就上网搜索找了一些整理下。

如果你只是转换单个文件，推荐直接在线转换：[在线互转地址](http://www.atool9.com/html2markdown.php)

其实这个在线地址里面有好多在线工具，需要的自己研究吧：  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191128113313528.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjA4MTM4OQ==,size_16,color_FFFFFF,t_70)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191128113535233.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjA4MTM4OQ==,size_16,color_FFFFFF,t_70)

# 二、互转模块：

## 1、md转html

这里我找到俩个模块可以使用，但是[md-to-html](https://pypi.org/project/md-to-html/)模块效果不好，网上大多数使用的也是[markdown](https://pypi.org/project/Markdown/)的模块。

### ①、markdown模块（推荐）：

这里有一个我感觉还不错的[博客地址](https://www.smslit.top/2018/10/16/md2html_python/)，感兴趣的可以去学习看看：  
第一步：下载安装：[markdown](https://pypi.org/project/Markdown/)

第二步：准备一个md文件，我这里是使用CSDN写作的部分帮助文档md。

```markdown

    快捷键
    ---------------------------
    撤销：Ctrl/Command + Z
    重做：Ctrl/Command + Y
    加粗：Ctrl/Command + B
    斜体：Ctrl/Command + I
    标题：Ctrl/Command + Shift + H
    无序列表：Ctrl/Command + Shift + U
    有序列表：Ctrl/Command + Shift + O
    检查列表：Ctrl/Command + Shift + C
    插入代码：Ctrl/Command + Shift + K
    插入链接：Ctrl/Command + Shift + L
    插入图片：Ctrl/Command + Shift + G
    查找：Command + F
    替换：Command + G
    
    标题
    ---------------------------
    # 1级标题
    ## 2级标题
    ### 3级标题
    #### 四级标题 
    ##### 五级标题  
    ###### 六级标题
    
    文本样式
    ---------------------------
    *强调文本* _强调文本_
    
    **加粗文本** __加粗文本__
    
    ==标记文本==
    
    ~~删除文本~~
    
    > 引用文本
    
    H~2~O is是液体。
    
    2^10^ 运算结果是 1024。
    
    列表
    ---------------------------
    - 项目
      * 项目
        + 项目
    
    1. 项目1
    2. 项目2
    3. 项目3
    
    - [ ] 计划任务
    - [x] 完成任务
    
    链接
    ---------------------------
    链接: [link](https://mp.csdn.net).
    
    图片: ![Alt](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9hdmF0YXIuY3Nkbi5uZXQvNy83L0IvMV9yYWxmX2h4MTYzY29tLmpwZw)
    
    带尺寸的图片: ![Alt](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9hdmF0YXIuY3Nkbi5uZXQvNy83L0IvMV9yYWxmX2h4MTYzY29tLmpwZw =30x30)
    
    居中的图片: ![Alt](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9hdmF0YXIuY3Nkbi5uZXQvNy83L0IvMV9yYWxmX2h4MTYzY29tLmpwZw#pic_center)
    
    居中并且带尺寸的图片: ![Alt](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9hdmF0YXIuY3Nkbi5uZXQvNy83L0IvMV9yYWxmX2h4MTYzY29tLmpwZw#pic_center =30x30)
    
    
    

```
第三步：可以直接复制测试：

```python

    from markdown import markdown
    
    print(dir(markdown))
    file = open('help.md','r',encoding='utf-8').read()
    
    html = markdown(file)
    print(html)
    
    
    with open('ret.html', 'w', encoding='utf-8') as file:
        file.write(html)
    
    
    
    

```
然后看出输入的ret.html文件。  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191128112813299.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjA4MTM4OQ==,size_16,color_FFFFFF,t_70)

### ②、md-to-html模块（不推荐）：

[模块地址：](https://pypi.org/project/md-to-html/)  
第一步:安装md-to-html：

```bash

    pip install md-to-html
    

```
第二步：准备一个gbk的md文件，我直接使用刚刚的会报编码错误，然后桌面新建一个dbk的txt就可以了，但是转换效果极差。

第三步：

cmd或者powershell中执行命令：

```bash
     md-to-html -i .\help.txt -o .\ret2.html
    

```
结果就是：  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191128114156449.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjA4MTM4OQ==,size_16,color_FFFFFF,t_70)  
发现没有，笔者刚刚的转换少了很多东西，所以这个模块做的效果不好，不建议使用这个，推荐使用第一个模块。  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191128114021640.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjA4MTM4OQ==,size_16,color_FFFFFF,t_70)

## 2、html转md：

### ①、tomd模块：

[模块地址。](https://pypi.org/project/tomd/)  
安装：

```bash

    pip install tomd
    

```
第一步：使用刚刚成的html或者导出自己的一份博客的HTML文件进行测试。  
这里我贴下我刚刚转存成功的。

```html

    <h2>快捷键</h2>
    <p>撤销：Ctrl/Command + Z
    重做：Ctrl/Command + Y
    加粗：Ctrl/Command + B
    斜体：Ctrl/Command + I
    标题：Ctrl/Command + Shift + H
    无序列表：Ctrl/Command + Shift + U
    有序列表：Ctrl/Command + Shift + O
    检查列表：Ctrl/Command + Shift + C
    插入代码：Ctrl/Command + Shift + K
    插入链接：Ctrl/Command + Shift + L
    插入图片：Ctrl/Command + Shift + G
    查找：Command + F
    替换：Command + G</p>
    <h2>标题</h2>
    <h1>1级标题</h1>
    <h2>2级标题</h2>
    <h3>3级标题</h3>
    <h4>四级标题</h4>
    <h5>五级标题</h5>
    <h6>六级标题</h6>
    <h2>文本样式</h2>
    <p><em>强调文本</em> <em>强调文本</em></p>
    <p><strong>加粗文本</strong> <strong>加粗文本</strong></p>
    <p>==标记文本==</p>
    <p>~~删除文本~~</p>
    <blockquote>
    <p>引用文本</p>
    </blockquote>
    <p>H~2~O is是液体。</p>
    <p>2^10^ 运算结果是 1024。</p>
    <h2>列表</h2>
    <ul>
    <li>项目</li>
    <li>
    <p>项目</p>
    <ul>
    <li>项目</li>
    </ul>
    </li>
    <li>
    <p>项目1</p>
    </li>
    <li>项目2</li>
    <li>
    <p>项目3</p>
    </li>
    <li>
    <p>[ ] 计划任务</p>
    </li>
    <li>[x] 完成任务</li>
    </ul>
    <h2>链接</h2>
    <p>链接: <a href="https://mp.csdn.net">link</a>.</p>
    <p>图片: <img alt="Alt" src="https://imgconvert.csdnimg.cn/aHR0cHM6Ly9hdmF0YXIuY3Nkbi5uZXQvNy83L0IvMV9yYWxmX2h4MTYzY29tLmpwZw" /></p>
    <p>带尺寸的图片: <img alt="Alt" src="https://imgconvert.csdnimg.cn/aHR0cHM6Ly9hdmF0YXIuY3Nkbi5uZXQvNy83L0IvMV9yYWxmX2h4MTYzY29tLmpwZw =30x30" /></p>
    <p>居中的图片: <img alt="Alt" src="https://imgconvert.csdnimg.cn/aHR0cHM6Ly9hdmF0YXIuY3Nkbi5uZXQvNy83L0IvMV9yYWxmX2h4MTYzY29tLmpwZw#pic_center" /></p>
    <p>居中并且带尺寸的图片: <img alt="Alt" src="https://imgconvert.csdnimg.cn/aHR0cHM6Ly9hdmF0YXIuY3Nkbi5uZXQvNy83L0IvMV9yYWxmX2h4MTYzY29tLmpwZw#pic_center =30x30" /></p>
    

```
第二步：  
转换代码：  
其中：ret.html就是上面的html，make.md就是转换成功的markdown文件。

```python

    from tomd import Tomd
    
    md_text = open('ret.html', 'r', encoding='utf-8').read()
    markdown = Tomd(md_text).markdown
    with open('make.md', 'w', encoding='utf-8') as file:
        file.write(markdown)
    
    

```
成功的md文件：

```markdown

    ## 快捷键
    
    撤销：Ctrl/Command + Z
    重做：Ctrl/Command + Y
    加粗：Ctrl/Command + B
    斜体：Ctrl/Command + I
    标题：Ctrl/Command + Shift + H
    无序列表：Ctrl/Command + Shift + U
    有序列表：Ctrl/Command + Shift + O
    检查列表：Ctrl/Command + Shift + C
    插入代码：Ctrl/Command + Shift + K
    插入链接：Ctrl/Command + Shift + L
    插入图片：Ctrl/Command + Shift + G
    查找：Command + F
    替换：Command + G
    
    ## 标题
    
    # 1级标题
    
    ## 2级标题
    
    ### 3级标题
    
    #### 四级标题
    
    ##### 五级标题
    
    ###### 六级标题
    
    ## 文本样式
    
    **强调文本** **强调文本**
    
    **加粗文本** **加粗文本**
    
    ==标记文本==
    
    ~~删除文本~~
    
    > 
    引用文本
    
    
    H~2~O is是液体。
    
    2^10^ 运算结果是 1024。
    
    ## 列表
    
    - 项目
    <li>
    项目
    <ul>
    - 项目
    
    项目1
    
    项目3
    
    [ ] 计划任务
    
    ## 链接
    
    链接: [link](https://mp.csdn.net).
    
    图片: <img alt="Alt" src="https://imgconvert.csdnimg.cn/aHR0cHM6Ly9hdmF0YXIuY3Nkbi5uZXQvNy83L0IvMV9yYWxmX2h4MTYzY29tLmpwZw" />
    
    带尺寸的图片: <img alt="Alt" src="https://imgconvert.csdnimg.cn/aHR0cHM6Ly9hdmF0YXIuY3Nkbi5uZXQvNy83L0IvMV9yYWxmX2h4MTYzY29tLmpwZw =30x30" />
    
    居中的图片: <img alt="Alt" src="https://imgconvert.csdnimg.cn/aHR0cHM6Ly9hdmF0YXIuY3Nkbi5uZXQvNy83L0IvMV9yYWxmX2h4MTYzY29tLmpwZw#pic_center" />
    
    居中并且带尺寸的图片: <img alt="Alt" src="https://imgconvert.csdnimg.cn/aHR0cHM6Ly9hdmF0YXIuY3Nkbi5uZXQvNy83L0IvMV9yYWxmX2h4MTYzY29tLmpwZw#pic_center =30x30" />
    
    

```
### ②、html2text文件（推荐）：

[模块地址：](https://pypi.org/project/html2text/)

安装模块：

```bash

    pip install html2text
    

```
转换代码：

```python

    import html2text
    
    
    md_text = open('ret.html', 'r', encoding='utf-8').read()
    
    markdown = html2text.html2text(md_text)
    with open('make2.md', 'w', encoding='utf-8') as file:
        file.write(markdown)
    
    

```
最后生成一个make2.md文件。

```

    ## 快捷键
    
    撤销：Ctrl/Command + Z 重做：Ctrl/Command + Y 加粗：Ctrl/Command + B 斜体：Ctrl/Command +
    I 标题：Ctrl/Command + Shift + H 无序列表：Ctrl/Command + Shift + U 有序列表：Ctrl/Command
    + Shift + O 检查列表：Ctrl/Command + Shift + C 插入代码：Ctrl/Command + Shift + K
    插入链接：Ctrl/Command + Shift + L 插入图片：Ctrl/Command + Shift + G 查找：Command + F
    替换：Command + G
    
    ## 标题
    
    # 1级标题
    
    ## 2级标题
    
    ### 3级标题
    
    #### 四级标题
    
    ##### 五级标题
    
    ###### 六级标题
    
    ## 文本样式
    
    _强调文本_ _强调文本_
    
    **加粗文本** **加粗文本**
    
    ==标记文本==
    
    ~~删除文本~~
    
    > 引用文本
    
    H~2~O is是液体。
    
    2^10^ 运算结果是 1024。
    
    ## 列表
    
      * 项目
      * 项目
    
        * 项目
      * 项目1
    
    项目2
    
      * 项目3
    
      * [ ] 计划任务
    
      * [x] 完成任务
    
    ## 链接
    
    链接: [link](https://mp.csdn.net).
    
    图片:
    ![Alt](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9hdmF0YXIuY3Nkbi5uZXQvNy83L0IvMV9yYWxmX2h4MTYzY29tLmpwZw)
    
    带尺寸的图片:
    ![Alt](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9hdmF0YXIuY3Nkbi5uZXQvNy83L0IvMV9yYWxmX2h4MTYzY29tLmpwZw
    =30x30)
    
    居中的图片:
    ![Alt](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9hdmF0YXIuY3Nkbi5uZXQvNy83L0IvMV9yYWxmX2h4MTYzY29tLmpwZw#pic_center)
    
    居中并且带尺寸的图片:
    ![Alt](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9hdmF0YXIuY3Nkbi5uZXQvNy83L0IvMV9yYWxmX2h4MTYzY29tLmpwZw#pic_center
    =30x30)
    
    
    

```
### ③、html2markdown模块：

[模块地址：](https://pypi.org/project/html2markdown/)

安装：

```bash
    pip install html2markdown
```
转换代码：

```python

    import html2markdown
    
    
    md_text = open('ret.html', 'r', encoding='utf-8').read()
    
    markdown = html2markdown.convert(md_text)
    
    with open('make3.md', 'w', encoding='utf-8') as file:
        file.write(markdown)
    

```
最后生成md文件：

```

    ## 快捷键
    
    撤销：Ctrl/Command + Z重做：Ctrl/Command + Y加粗：Ctrl/Command + B斜体：Ctrl/Command + I标题：Ctrl/Command + Shift + H无序列表：Ctrl/Command + Shift + U有序列表：Ctrl/Command + Shift + O检查列表：Ctrl/Command + Shift + C插入代码：Ctrl/Command + Shift + K插入链接：Ctrl/Command + Shift + L插入图片：Ctrl/Command + Shift + G查找：Command + F替换：Command + G
    
    ## 标题
    
    # 1级标题
    
    ## 2级标题
    
    ### 3级标题
    
    #### 四级标题
    
    ##### 五级标题
    
    ###### 六级标题
    
    ## 文本样式
    
    _强调文本_ _强调文本_
    
    __加粗文本__ __加粗文本__
    
    ==标记文本==
    
    ~~删除文本~~
    
    >  
    > 引用文本
    > 
    
    H~2~O is是液体。
    
    2^10^ 运算结果是 1024。
    
    ## 列表
    
    *   项目
    *   
        
        项目
        
        
        
        *   项目
        
        
        
    *   
        
        项目1
        
        
    *   项目2
    *   
        
        项目3
        
        
    *   
        
        \[ \] 计划任务
        
        
    *   \[x\] 完成任务
    
    ## 链接
    
    链接: [link](https://mp.csdn.net).
    
    图片: ![Alt](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9hdmF0YXIuY3Nkbi5uZXQvNy83L0IvMV9yYWxmX2h4MTYzY29tLmpwZw)
    
    带尺寸的图片: ![Alt](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9hdmF0YXIuY3Nkbi5uZXQvNy83L0IvMV9yYWxmX2h4MTYzY29tLmpwZw =30x30)
    
    居中的图片: ![Alt](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9hdmF0YXIuY3Nkbi5uZXQvNy83L0IvMV9yYWxmX2h4MTYzY29tLmpwZw#pic_center)
    
    居中并且带尺寸的图片: ![Alt](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9hdmF0YXIuY3Nkbi5uZXQvNy83L0IvMV9yYWxmX2h4MTYzY29tLmpwZw#pic_center =30x30)
    

```
通过对比三个模块的markdown文件的结果，发现生成的效果第二个模块的我感觉还不错（对比项目那一块的结果，其他的基本上都一样。）

