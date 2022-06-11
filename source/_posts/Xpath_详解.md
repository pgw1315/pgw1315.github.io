---
title: Xpath_详解
date: 2022-06-12 05:30:43
author: Will
img: /images/banner/XPath.jpeg
categories: Python
tags:
  - Python
  - Xpath
---
        
## 什么是 Xpath？

Xpath 是一种用在 XML 文档中定位元素的语言，同样也支持 HTML 元素的解析。

所谓 Xpath，是指 XML path language。path 就是路径，那么 Xpath 主要是通过路径来查找元素。

我们通过下面一张小图来了解一下 HTML 中的结构：

![](/images/Xpath_详解/8178525-1d5d648e03370891.png)

HTML 的结构就是树形结构，HTML 是根节点，所有的其他元素节点都是从根节点发出的。其他的元素都是这棵树上的节点`Node`，每个节点还可能有属性和文本。
+ 而路径就是指某个节点到另一个节点的路线。

节点之间存在各种关系：

+ 父节点(Parent)： HTML 是 body 和 head 节点的父节点；

+ 子节点(Child)：head 和 body 是 HTML 的子节点；

+ 兄弟节点(Sibling)：拥有相同的父节点，head 和 body 就是兄弟节点。title 和 div 不是兄弟，因为他们不是同一个父节点。

+ 祖先节点(Ancestor)：body 是 form 的祖先节点，爷爷辈及以上👴；

+ 后代节点(Descendant)：form 是 HTML 的后代节点，孙子辈及以下👶。

Xpath 中的绝对路径从 HTML 根节点开始算，相对路径从任意节点开始。

通过开发者工具，我们可以拷贝到 Xpath 的绝对路径和相对路径代码：

![](/images/Xpath_详解/8178525-3ce3a8737955cffe.png)

但是由于拷贝出来的代码缺乏灵活性，也不全然准确。大部分情况下，都需要自己定义 Xpath 语句，因此 Xpath 语法还是有必要学习。

## 绝对路径：

Xpath 中最直观的定位策略就是绝对路径。

以百度中的输入框和按钮为例，通过拷贝出来的 full Xpath：

```python
/html/body/div[2]/div/div/div/div/form/span/input

```

这就是一个绝对路径我们可以看出，绝对路径是从根节点`/html`开始往下，一层层的表示出来直到需要的节点为止。

这明显不是理智的选项，因此了解以下即可，不用往心里去。

## 相对路径

除了绝对路径，Xpath 中更常用的方式是相对路径定位方法，以“//”开头。

相对路径可以从任意节点开始，一般我们会选取一个可以唯一定位到的元素开始写，可以增加查找的准确性。

相对路径可以通过以下的方式来定位元素：

### 基本定位语法

定位语法主要依赖于以下特殊符号：

|表达式|说明|举例|
| --- |--- |--- |
 | `/` | 从根节点开始选取 | /html/div/span|
 | `//` | 从任意节点开始选取 | //input|
 | `.` | 选取当前节点 | |
 | `..` | 选取当前节点的父节点 | //input/.. 会选取 input 的父节点|
 | `@` | 选取属性，或者根据属性选取 | //input[@data] 选取具备 data 属性的 input 元素 //@data 选取所有 data 属性|
 | `*` | 通配符，表示任意节点或任意属性 | |

### 元素属性定位

属性定位是通过 @ 符号指定需要使用的属性。

+ 根据元素是否具备某个属性查找元素

```python
//*[@data-recordid]

```

选取包含data-recordid属性的所有节点。可以定位到以下元素：

```python
<tr role="row" data-boundview="gridview-1029" data-recordid="B36BCA33" ></tr>

```

+ 根据属性是否等于某值查找元素

```python
//span[@role='img']

```

选取属性 role 的属性值为 img 的所有节点。可以定位到以下元素：

```python
<span role="img" class="x-btn-icon-el" unselectable="on" style=""></span>

```

注意，属性值必须要加引号，单双引号都可以。

### 层级属性结合定位

遇到某些元素无法精确定位的时候，可以查找其父级及其祖先节点，找到有确定的祖先节点后通过层级依次向下定位。

以下面的结构为例：

+ 
![](/images/Xpath_详解/8178525-1187d4bb553f53c6.png)

```python
<form action="search" id="form" method="post">
    <span class="bg">
        <span class="soutu">搜索</span>
    </span>
    <span class="soutu">
        <input type="text" name="key" id="su">
    </span>
</form>

```

+ 根据层级向下找，从 form 找到绿色的 span：

```python
//form[@id="form"]/span/span

```

+ 查找某元素内部的所有元素，选取 form 元素内部的所有 span：

```python
//form[@id="form"]//span

```

第二个双斜杠，表示选取内部所有的 span，不管层级关系

+ 使用星号找不特定的元素

```python
//*[@id="form"]//*[@type="text"]

```

选取 id 属性为 form 的任意属性内部，并且 type 属性为 text 的任意元素。这里会找到 input。

+ 使用`..`从下往上找，根据 input 查找其父节点 span：

```python
//input[@name="key"]/..

```

注意最后的两个点，找到 input 节点的上级节点，如果还要再往上再加 `/..`

+ 找同级节点：
+ 比如我们想通过第一个 span 标签去 找 div 标签。树形结构中，兄弟节点之间的关系是通过父节点建立起来的。所以可以先找到父节点，再通过父节点找同级节点。

```python
//span[@class="bg"]/../div

```

先通过`/..`找到 span 的父节点，再通过父节点找到 div。

### 使用谓语定位

谓语是 Xpath 中用于描述元素位置。主要有数字下标、最后一个子元素`last()`、元素下标函数`position()`。

+ 使用下标的方式，从 form 找到 input ：

```python
//form[@id="form"]/span[2]/input

```

Xpath 中的下标从 `1` 开始。

+ 查找最后一个子元素，选取 form 下的最后一个 span：

```python
//form[@id="form"]/span[last()]

```

+ 查找倒数第几个子元素，选取 form 下的倒数第一个 span：

```python
//form[@id="form"]/span[last()-1]

```

+ 使用 position() 函数，选取 from 下第二个 span：

```python
//form[@id="form"]/span[position()=2]

```

+ 使用 position() 函数，选取下标大于 2 的 span：

```python
//form[@id="form"]/span[position()>2]

```

### 使用逻辑运算符

如果元素的某个属性无法精确定位到这个元素，我们还可以用逻辑运算符 and 连接多个属性进行定位，以百度输入框为例。

+ 使用 `and` ：

```python
//*[@name='wd' and @class='s_ipt']

```

查找 name 属性为 wd 并且 class 属性为 s_ipt 的任意元素

+ 使用 `or`：

```python
//*[@name='wd' or @class='s_ipt']

```

查找 name 属性为 wd 或者 class 属性为 s_ipt 的任意元素，取其中之一满足即可。

+ 使用`|`，同时查找多个路径，取或：

```python
//form[@id="form"]//span | //form[@id="form"]//input

```

选取 form 下所有的 span 和所有的 input。

### 使用文本定位

使用文本定位，是 Xpath 中的一大特色。在自动化测试中，为了让代码的可读性更高，可以使用文本的方式。

以下一个案例：

![](/images/Xpath_详解/8178525-466743dbeb77cb6f.png)

部分网页结构如下：

```python
<tr>
  <td valign="top">
    <input type="radio" name="payment" value="1" checked="" iscod="0">
  </td>
  <td valign="top">
    <strong>支付宝</strong>
  </td>
</tr>

```

其实我们需要点的是前的单选框，但是单选框没有任何特殊的标识，不够灵活。我们可以通过后面的名称，如(支付宝、余额支付等)来找到其对应行的 radio，再去点击。

我们就需要先通过文本定位到“支付宝”，再去找其同一行(tr)的 input 节点。如果理不清楚，我们可以先画一个结构图：

![](/images/Xpath_详解/8178525-443ad23bb513cce3.png)

红色箭头表示查找路径，先定位到“支付宝”所在的 strong，再定位 `td -> tr -> td - >input` 。那么要定位“支付宝”文本，就需要用到 Xpath 中的函数 `text()` 或 `string()`，注意是函数，所以括号不能少。

`text()`：当前元素节点包含的文本内容；
+ 表达式`//div[text()="文本"]`，能找到：

```python
<div>文本</div>

```

不能找到：

```python
<div><span>文本</span></div>

```

`string()`：当前元素节点内部所有节点元素的文本内容。表达式`//div[string()="文本"]`上述两种情况都能找到。

好，接着写上面的内容。先通过 `//strong[text()="支付宝"]`定位到“支付宝”所在的元素 strong，再找上级 td -> tr，再向下找：

```python
//strong[text()="支付宝"]/../../td[1]/input

```

也可以写为：

```python
//td[string()="支付宝"]/../td[1]/input

```

### 使用部分匹配函数

Xpath 中有提供了几个函数，用来进行部分匹配。

|函数|说明|举例|
| --- |--- |--- |
| contains | 选取属性或者文本包含某些字符 | //div[contains(@id, 'data')] 选取 id 属性包含 data 的 div 元素  //div[contains(string(), '支付宝')] 选取内部文本包含“支付宝”的 div 元素|
| starts-with | 选取属性或者文本以某些字符开头 | //div[starts-with(@id, 'data')] 选取 id 属性以 data 开头的 div 元素  //div[starts-with(string(), '银联')] 选取内部文本以“银联”开头的 div 元素|
| ends-with | 选取属性或者文本以某些字符开头 | //div[ends-with(@id, 'require')] 选取 id 属性以 require 结尾的 div 元素  //div[ends-with(string(), '支付')] 选取内部文本以“支付”结尾的 div 元素|

## 验证 Xpath

验证 xpath 有两种方法：

+ 在开发者工具的 Elements 中按`Ctrl + F`，在搜索框中输入 Xpath：

![](/images/Xpath_详解/8178525-a9ed19d1dffab883.png)

+ 在开发者工具的 Console 中使用 `$x()`

![](/images/Xpath_详解/8178525-1d6c86e9e1723c6a.png)

## 参考文章
[Xpath_详解](https://www.jianshu.com/p/6a0dbb4e246a)