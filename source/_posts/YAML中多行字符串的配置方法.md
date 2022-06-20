---
title: YAML中多行字符串的配置方法
date: 2022-06-18 13:31:34
author: Will
img: /images/banner/yaml.png
categories: 其他
tags:
  - YAML
---
        
有时候我们会在配置文件中配置一段文字说明，这种时候通常会出现两种需求：
+ 文字中可能出现段落，希望在配置中按段落方式编写，显示打印的时候也能出现段落换行。
+ 文字很长，为方便编辑，可能在配置文件中分段写，但是显示的时候不喜欢出现配置中的段落换行。

简单的说，就是：

+ 配置与显示，都严格按段落展示
+ 配置按段落，显示不需要按段落

假设，我们需要配置这样一段文字：

```yaml
I am a coder.My blog is didispace.com.复制代码
```

下面，就针对上面的两种情况来看看可以怎么来实现：

### 配置与显示，都严格按段落展示

这个需求下，我们希望配置和显示都按句子换行，就是这样：

```yaml
I am a coder.
My blog is didispace.com.复制代码
```

#### 方法一：直接使用`\n`来换行

这样写：

```yaml
string: "I am a coder.\n\
         My blog is didispace.com."复制代码
```

最终输出：

```yaml
I am a coder.
My blog is didispace.com.复制代码
```

通过`\n`在显示的时候换行，通过配置行末的`\`让这个字符串换行继续写（这个必须有，如果没有第二行行首会多一个空格）。

**注意**：这里必须使用双引号来定义字符串，不能用单引号。因为单引号是不支持`\n`换行的。

#### 方法二：使用`｜`、`｜+`、`｜-`

在方法一种，其实我们在文字中加入了几个转义符号，其实对于阅读并不方便。在方法二中，将介绍更适合阅读的几种形式：

```yaml
string: |
  I am a coder.
  My blog is didispace.com.

string: |+
  I am a coder.
  My blog is didispace.com.

string: |-
  I am a coder.
  My blog is didispace.com.复制代码
```

如上面一共有三种配置都会自动按配置中所写的换行来换行，但是在文末会有一些区别，有的会增加一个空行，有的不会，有的会新增两个空行，具体说明如下：

+ `|`：文中自动换行 + 文末新增一空行

+ `|+`：文中自动换行 + 文末新增两空行

+ `|-`：文中自动换行 + 文末不新增行

### 配置按段落，显示不需要按段落

这个需求下，我们希望配置里是按行写的，但是显示是如下面这样在一行的：

```yaml
I am a coder.My blog is didispace.com.复制代码
```

#### 方法一：直接在字符串中换行写

最粗暴的写法，反正不用换行，那就直接写了：

```yaml
string: 'I am a coder.
         My blog is didispace.com.'复制代码
```

这里不论用双引号还是单引号都是可以的。因为不存在需要转移的内容，所以总体还算清晰。

#### 方法二：使用`>`、`>+`、`>-`

比较好的表述方式就是使用`>`、`>+`、`>-`来定义，比如下面这几种：

```yaml
string: >
  I am a coder.
  My blog is didispace.com.

string: >+
  I am a coder.
  My blog is didispace.com.

string: >-
  I am a coder.
  My blog is didispace.com.复制代码
```

这三种都不会对配置中的换行进行实际换行，但是依然在文末的处理会有一些小区别，具体如下：

+ `>`：文中不自动换行 + 文末新增一空行

+ `>+`：文中不自动换行 + 文末新增两空行

+ `>-`：文中不自动换行 + 文末不新增行

### 参考文章
[YAML中多行字符串的配置方法](https://juejin.cn/post/6844903972688363534)