---
title: Python中的字符串详解
date: 2022-06-13 12:33:43
author: Will
img: /images/banner/python_string.jpeg
categories: Python
tags:
  - Python
---
        

### Python中的字符串

字符串（String）是字符序列，或者说是一串字符。
字符只是一个符号。例如，英语具有26个字符。Python 不支持单字符类型，单字符在 Python 中也是作为一个字符串使用。
通过将字符括在单引号或双引号中来创建字符串。Python中甚至可以使用三引号，但通常用于表示多行字符串和文档字符串。
```python
varS = 'Hello World!'

三引号字符串可以扩展多行

my_str = """Hello, welcome to

           the world of Python"""

```

Python中的字符串索引:（正）索引从0开始；Python允许负索引，-1 为从末尾的开始位置。

参见下图：
![](/images/Python中的字符串详解/20210907161243809.png)

可以使用方括号 [] 来标识其中的字符，也可用来截取字符串，
标识其中的元素格式如：变量[下标]
字符串的切片（截取字符串的一段）语法格式如：变量[头下标:尾下标]，冒号是切片运算符。注意，切片结果不含尾下标字符，遵循左闭右开原则。

参见如下的例子：
![](/images/Python中的字符串详解/20210907161330157.png)

#### 使用字符串需要注意:

☆包含字符串引号必须一致，不能一边是单引号一边是双引号，如：var1="good' 将报错。

![](/images/Python中的字符串详解/20210910182031229.png)

☆当字符串中包含单引号（'）使用双引号创建字符串；字符串包含双引号（"）使用单引号创建。如
```python
var2 = "I'm just a student"
var3 = 'What is "Python"?'

```
![](/images/Python中的字符串详解/20210910182031274.png)

也可以使用转移字符转义字符，如：
```python
var4 = 'I\'m just a student'
```

![](/images/Python中的字符串详解/20210910182031275.png)

关于转义字符，后面将进一步介绍。

☆字符串是不可变数据类型，也就是说你要改变原字符串内的元素，只能是新建另一个字符串。换句话说， 字符串是不可变的。这意味着字符串的元素一旦分配就无法更改。我们可以简单地将不同的字符串重新分配给相同的名称。

不能删除或删除字符串中的字符。但是使用del关键字可以完全删除字符串。
例如删除my_string = 'Python'，可用如下语句：

```python
del my_string

```

### Python字符串操作

Python字符串运算符

下表示例中，变量 a 值为字符串 "Hello"，b 变量值为 "Python"：

| **操作符**  |  **描述**  |  **实例** |
| --- |--- |--- |
|  +  | 字符串连接(也称为拼接)  |  a + b 输出结果： HelloPython | 
| *  |  重复输出字符串  |  a*2 输出结果：HelloHello | 
| []  |  通过索引获取字符串中字符  |  a[1] 输出结果** e** | 
| [ : ]  |  截取字符串中的一部分，遵循**左闭右开**原则，str[0:2] 是不包含第 3 个字符的。  | a[1:4] 输出结果 ell | 
| in  |  成员运算符 - 如果字符串中包含给定的字符返回 True  |  'H' in a 输出结果 True | 
| not in  |  成员运算符 - 如果字符串中不包含给定的字符返回 True  |  'M' not in a 输出结果 True 

#### python里的原始字符串

原始字符串：所有的字符串都是直接按照字面的意思来使用。
python原始字符串是指在引号前添加 r 或 R 前缀的字符串，在原始字符串中，字符“\” 不再表示转义字符的含义。
如：
直接使用‘c:\now'会报错，\需要转义，应该这样写 'c:\\now'，或者采用原始字符串的写法r‘c:\now'
对于 ASCII 编码，0~31（十进制）范围内的字符为控制字符，它们都是看不见的，不能在显示器上显示，甚至无法从键盘输入，只能用转义字符的形式来（以反斜杠\开始）表示，常见的转义字符参见下表：

|  **转义字符**  |  **说明**  |  **例子** |
| --- |--- |--- |
|  \n  |  换行符，将光标位置移到下一行开头。  | print("Hello\nWorld!")  输出如下： Hello World! | 
|  \t  |  水平制表符，也即 Tab 键，一般相当于四个空格。  |  print("Hello\tWorld!")  输出如下： Hello        World! | 
|  \\  | 反斜线  |  print('此路径是D:\\test\\hello.txt') 输出如下： 此路径是D:\test\hello.txt |
|  \'  |  单引号  |  print('I\'m Li Ning') 输出如下： I'm Li Ning | 
|  \"  |  双引号  |  print("可以使用\"创建字符串") 输出如下： 可以使用"创建字符串 |
|  \  |  在字符串行尾的续行符，即一行未完，转到下一行继续写。  |  print("line1 \ line2 \ line3") 输出如下： line1 line2 line3 |

#### f字符串（f-string）

以f开头的字符串，称之为f-string，是从python3.6引入的它和普通字符串不同之处在于，字符串如果包含{xxx}，就会以对应的变量替换：
```python
r = 2.5
s = 3.14 * r ** 2
print(f'半径为{r}的圆的面积为{s:.2f}') #半径为2.5的圆的面积为19.62
```
参见下图：
![](/images/Python中的字符串详解/2021091015555711.png)

上述代码中，{r}被变量r的值替换，{s:.2f}被变量s的值替换，并且:后面的.2f指定了格式化参数（即保留两位小数），因此，{s:.2f}的替换结果是19.62。

f字符串的前缀为f，{}括号部分是被替换值，其中冒号前是变量名，冒号后指定用于类型，填充或对齐的格式说明符。

#### Python 的字符串常用方法（函数）

【详见官网[内置类型 — Python 3.9.7 文档](https://docs.python.org/zh-cn/3/library/stdtypes.html#string-methods) 】

方法及描述概述如下：

+ capitalize()
将字符串的第一个字符转换为大写，不会修改原始字符串。

![](/images/Python中的字符串详解/20210908111308767.png)

+ casefold()

把字符串大写字符转换为小写。不会修改原始字符串。

+ center(width, fillchar)

返回一个指定的宽度 width 居中的字符串，fillchar 为填充的字符，默认为空格。

+ count(str, beg= 0,end=len(string))

返回 str 在 string 里面出现的次数，如果 beg 或者 end 指定则返回指定范围内 str 出现的次数。

字符索引值从0开始。

![](/images/Python中的字符串详解/20210908161802542.png)

+ encode(encoding='UTF-8',errors='strict')

以 encoding 指定的编码格式编码字符串，如果出错默认报一个ValueError 的异常，除非 errors 指定的是'ignore'或者'replace'。

+ endswith(suffix, beg=0, end=len(string))

检查字符串是否以 obj 结束，如果beg 或者 end 指定则检查指定的范围内是否以 obj 结束，如果是，返回 True,否则返回 False。

+ expandtabs(tabsize=8)

把字符串 string 中的 tab 符号转为空格，tab 符号默认的空格数是 8 。

+ find(str, beg=0, end=len(string))

检测 str 是否包含在字符串中，如果指定范围 beg（开始） 和 end（结束） ，则检查是否包含在指定范围内，如果包含返回开始的索引值，否则返回-1。

![](/images/Python中的字符串详解/20210911090100608.png)

+ index(str, beg=0, end=len(string))

跟find()方法一样，只不过如果str不在字符串中会报一个异常。

+ isalnum()

如果字符串至少有一个字符并且所有字符都是字母或数字则返 回 True，否则返回 False。

+ isalpha()

如果字符串至少有一个字符并且所有字符都是字母或中文字则返回 True, 否则返回 False。

+ isdigit()

如果字符串只包含数字则返回 True 否则返回 False。

+ islower()

如果字符串中包含至少一个区分大小写的字符，并且所有这些(区分大小写的)字符都是小写，则返回 True，否则返回 False。

+ isnumeric()

如果字符串中只包含数字字符，则返回 True，否则返回 False。

+ isspace()

如果字符串中只包含空白，则返回 True，否则返回 False.

+ istitle()

如果字符串是标题化的(见 title())则返回 True，否则返回 False。

+ isupper()

如果字符串中包含至少一个区分大小写的字符，并且所有这些(区分大小写的)字符都是大写，则返回 True，否则返回 False。

+ join(seq)

以指定字符串作为分隔符，将 seq 指定要连接的元素序列连接生成一个新的字符串。例如：
```python
a = "We"
b = "learn"
c = "Python"
str = " ";
seq = (a, b, c); #字符串序列
print(str.join(seq)) #输出：We learn Python
```
运行效果如下：

![](/images/Python中的字符串详解/20210911082339166.png)

+ len(string)

返回字符串长度。

默认情况下，用 len(string) 函数计算该字符串的长度时，不区分英文、数字、汉字等字符，每个字符长度为1，例如：
```python
s = "我爱学Python"
```
![](/images/Python中的字符串详解/2021091316431797.png)

在实际应用中，有时需要获得字符串占用的字节数。这怎么办？在 Python 中，不同的字符所占的字节数不同，数字、英文字母、小数点、下划线以及空格，各占一个字节，而一个汉字可能占 2~4 个字节，具体占多少个，取决于采用的编码方式。例如，汉字在 GBK/GB2312 编码中占用 2 个字节，而在 UTF-8 编码中一般占用 3 个字节。可以通过encode()方法（函数），该方法默认按使用UTF-8编码计算，若按GBK或者GB2312计算，可以带参数，例如：

![](/images/Python中的字符串详解/20210913170452582.png)

+ ljust(width[, fillchar])

返回一个原字符串左对齐,并使用 fillchar 填充至长度 width 的新字符串，fillchar 默认为空格。

+ lower()

转换字符串中所有大写字符为小写。

+ lstrip([chars])

截掉字符串左边的空格或指定字符。chars -- 指定删除的字符（默认即缺省为空格）

+ maketrans()

创建字符映射的转换表，对于接受两个参数的最简单的调用方式，第一个参数是字符串，表示需要转换的字符，第二个参数也是字符串表示转换的目标。

+ max(str)

返回字符串 str 中最大的字母。

+ min(str)

返回字符串 str 中最小的字母。

+ replace(old, new [, max])

把 将字符串中的 old 替换成 new,如果 max 指定，则替换不超过 max 次。

+ rfind(str, beg=0,end=len(string))

类似于 find()函数，不过是从右边开始查找。

+ rindex( str, beg=0, end=len(string))

类似于 index()，不过是从右边开始。

+ rjust(width,[, fillchar])

返回一个原字符串右对齐,并使用fillchar(默认空格）填充至长度 width 的新字符串。

+ rstrip([chars])

删除字符串末尾的空格或指定字符。chars -- 指定删除的字符（默认即缺省为空格）

+ split(str="", num=string.count(str))

以 str 为分隔符截取字符串，如果 num 有指定值，则仅截取 num+1 个子字符串。

![](/images/Python中的字符串详解/2021091109162366.png)

+ splitlines([keepends])

按照行('\r', '\r\n', \n')分隔，返回一个包含各行作为元素的列表，如果参数 keepends 为 False，不包含换行符，如果为 True，则保留换行符。

+ startswith(substr, beg=0,end=len(string))

检查字符串是否是以指定子字符串 substr 开头，是则返回 True，否则返回 False。如果beg 和 end 指定值，则在指定范围内检查。

+ strip([chars])

在字符串上执行 lstrip()和 rstrip()，chars -- 指定删除的字符（默认即缺省为空格）

+ swapcase()

将字符串中大写转换为小写，小写转换为大写。

+ title()

返回"标题化"的字符串,就是说所有单词都是以大写开始，其余字母均为小写(见 istitle())。

+ translate(table, deletechars="")

根据 str 给出的表(包含 256 个字符)转换 string 的字符, 要过滤掉的字符放到 deletechars 参数中。

+ upper()

转换字符串中的小写字母为大写。

+ zfill (width)

返回长度为 width 的字符串，原字符串右对齐，前面填充0。

+ isdecimal()

检查字符串是否只包含十进制字符，如果是返回 true，否则返回 false。

函数print()和函数format()结合起来实现格式化输出，功能比较强大。

先给出基本示例：
```python
print('李振兴告诉{}说 "{}!"'.format('赵萌萌', '加油'))

print('{0}和{1}是好朋友！'.format('李振兴', '赵萌萌'))

print('{1}和{0}是好朋友！'.format('李振兴', '赵萌萌'))
```
花括号及之内的字符（称为格式字段）被替换为传递给 str.format()方法的对象。花括号中的可有数字，花括号中的数字表示传递给 str.format() 方法的对象所在的位置。

![](/images/Python中的字符串详解/2021091015564850.png)​

**format()函数，**可用于执行字符串格式化操作，更多情况，可参见 Python的输入函数input()和输出函数print()详解 [Python的输入函数input()和输出函数print()详解_cnds123的专栏-CSDN博客](https://blog.csdn.net/cnds123/article/details/118638607)

下面给出给出一个字符串拼接示例：

可以使用加号、字符串方法join、格式化、通过F-string拼接。源码如下：

```python
a = "We"
b = "learn"
c = "Python"
s1 = a + " " + b + " " + c #通过+号拼接
print(s1)

seq = (a, b, c); # 字符串序列
s2 = " ".join(seq)#通过 str.join()方法拼接
print(s2)

s3 = "{0} {1} {2}".format(a,b,c)  #格式化拼接
print(s3)

s4 = f"{a} {b} {c}" #通过F-string拼接，适用于python3.6.2+版本
print(s4)

```

运行之，参见下图：

![](/images/Python中的字符串详解/20210911090257164.png)

## 附录

Unicode 概述 [Unicode 指南 — Python 3.9.7 文档](https://docs.python.org/zh-cn/3/howto/unicode.html)

## 参考文章
[Python中的字符串详解](https://blog.csdn.net/cnds123/article/details/120160535)