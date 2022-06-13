---
title: Python的list列表详解
date: 2022-06-13 12:06:59
author: Will
img: /images/banner/pythonlist.jpeg
categories: Python
tags:
  - Python
---
        

Python list(列表)，是Python中最常用的一种数据结构，它是一组用方括号括起来、逗号分隔的数据。
列表的元素可以是任何类型的，但使用时通常各个元素的类型是相同的。
列表支持加入不同数据类型的元素：数字、字符串、列表、元组等。
列表是可变的，因此即使在创建后也可以对其进行更改。

Python中的列表是有序的，并且有一定数量。 根据确定的序列对列表中的元素进行索引，并使用0作为第一个索引来完成列表的索引。

#### 一、创建列表Creating a List

```python
list1 = ['Google', 'Runoob', 1997, 2000]
list2 = [1, 2, 3, 4, 5 ]
my_list = ['p','r','o','b','e']
kong = [] #创建空白list，用于后面调用
```

#### 二、访问列表中的元素Accessing elements from the List

我们可以使用索引运算符[]访问列表中的项。索引必须为整数。嵌套列表通过嵌套索引访问。

索引的排序规则如下图，从零开始。

![](/images/Python的list列表详解/4ded7b055733bb97a7b67097980065bd_a8859852-9dad-4113-a0d2-df554dcb9f07.png)

```python
List = ["Geeks", "For", "Geeks"] 
print(List[0])  
```

在使用索引访问列表中的元素时，使用的索引(下标)不可越界，否则会报如下错误 
+ 
IndexError: list index out of range

以下几种情况会引起越界：

+ 创建空的list，但使用list[0] 来访问其中元素

+ 访问list[n] ，但是N大于len(list)

+ 创建空的list，通过list[0]来赋值

#### 三、添加或删除元素Adding or Removing Elements

我们可以使用内置的append()函数将元素添加到列表中。 
通过使用append()方法，一次只能将一个元素添加到列表中，如果使用append()方法添加多个元素，则需要使用循环。

```python
List = [] 
print(List) 

# Addition of Elements  
List.append(1) 
List.append(2) 
print(List) 

# Adding elements to the List using Iterator 
for i in range(1, 4): 
    List.append(i) 
print(List) 
```

其运行结果如下

```python
[]
[1, 2]
[1, 2, 1, 2, 3]
```

append()方法仅适用于List末尾的元素添加，如果需要在中间位置插入元素，则使用insert()方法。 
insert()方法需要两个参数，位置和值。  insert(position, value)

```python
List = [1,2,3,4]
print(List)

List.insert(3, 12)
List.insert(0, 'Geeks')
print(List)
```

其运行结果如下

```python
[1, 2, 3, 4]
['Geeks', 1, 2, 3, 12, 4]
```

除了append()和insert()方法外，还有一个元素添加方法extend()，该方法用于在列表末尾同时添加多个元素。
我们可以使用内置的remove()函数从列表中删除元素，但是如果元素不存在于集合中，则会发生错误。 
Remove()方法一次只删除一个元素，如需删除多个需要使用循环语句。 
注意：remove(value)方法的参数是值，不是索引(index，下标)，此方法将仅删除搜索到的元素的第一个匹配项。

```python
List = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
print(List)

List.remove(5)
List.remove(6)
print(List)
```

运行结果如下

```python
List = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
print(List)

List.remove(5)
List.remove(6)
print(List)
```

### 四、列表合并Merge lists

前面我们说过，使用append()方法，可以在列表尾部添加一个新元素，所以对于两个或者多个列表的合并不是很适用。

```python
list1 = ['Google', 'Runoob', 1997, 2000]
list2 = [1, 2, 3, 4, 5 ]
list3 = [{'name': 'Soe', 'Sex': 'Female'}, {'name': 'John', 'Sex': 'Male'}]

newlist = list1.append(list2)
print(newlist)
```

大家猜猜返回什么结果？ 
+ 
None。可见list的许多方法，都是应用在list本身的，不能赋值到新的list，这一点要注意。

```python
list1.append(list2)
# Result: ['Google', 'Runoob', 1997, 2000, [1, 2, 3, 4, 5]]
list1.extend(list2)
# Result: ['Google', 'Runoob', 1997, 2000, 1, 2, 3, 4, 5]
```

上面这些只能合并两个list，也不方便，下面才是真正常用的方法

```python
newlist = list1 + list2 + list3
print(newlist)
# Result: ['Google', 'Runoob', 1997, 2000, 1, 2, 3, 4, 5, 1, 2, 3, 4, 5, {'name': 'Soe', 'Sex': 'Female'}, {'name': 'John', 'Sex': 'Male'}]

newlist = [*list1, *list2, *list3]
print(newlist)
# Result: ['Google', 'Runoob', 1997, 2000, 1, 2, 3, 4, 5, 1, 2, 3, 4, 5, {'name': 'Soe', 'Sex': 'Female'}, {'name': 'John', 'Sex': 'Male'}]

```

### 五、列表List的遍历以及判断元素是否在列表List中

通过for循环可以实现列表List的遍历

```python
a = ['a', 'b', 'c', 'd', 'e']

# simple iterate 简单遍历
for i in a:
    print(i)

# iterate with index 带索引的遍历
for i, el in enumerate(a):
    print(i, el)

# iterate with custom index 自定义索引号吗的遍历
for i, el in enumerate(a, 1):
    print(i, el)
```

通过in方法可以判断元素是否在列表中

```python
list1 = ['Google', 'Runoob', 1997, 2000]
if 'Google' in list1:
    print(True)
# Result: True
```

### 六、列表排序和去重去空Sort ,Remove duplicates, Remove NA

通过sort方法可以进行排序

```python
list1 = ['Google', 'Yahoo', 'Baidu', 'Sogou']
list1.sort(reverse=True)
print(list1)
# Result:  ['Yahoo', 'Sogou', 'Google', 'Baidu']
```

sort函数还有一个告警用法sort(key=Function)，我们在后续在探讨。 
使用先set后list的方法可以实现对列表list元素进行去重

```python
my_list = ['Google', 'Yahoo', 'Baidu', 'Sogou', 'Google']
newlist = list(set(my_list))
print(newlist)
# Result:  ['Baidu', 'Yahoo', 'Google', 'Sogou']
```

set()方法是创建一个集合，集合（set）是一个无序的不重复元素序列。创建完成集合set以后，在转换回来就还是一个列表list。 
除了去重，还有经常遇到去除空值的情况

```python
d = ['', '剧情', '喜剧', '恐怖', '', '伦理', '']
d_dropna = list(filter(None, d))    #去除列表空值
print(d_dropna)
# Result:  ['剧情', '喜剧', '恐怖', '伦理']
```

使用filter就可以过滤掉空值，然后在使用list将其格式转换为列表。

### 七、列表切片Slicing of a List

在Python列表中，有多种方法来打印包含所有元素的整个列表，但是要打印列表中特定范围的元素，我们使用切片操作。 

注意：除了列表List，元组Tuple，字符串string 也可以进行切片操作。

切片操作的格式如下

```python
list[start_index: end_index: step]
```

+ start：表示是第一个元素对象，正索引位置默认为0；负索引位置默认为 -序列长度 
+ end：表示是最后一个元素对象，正索引位置默认为 (序列长度－1)；负索引位置默认为 -1 
+ step：步长，end-start，步长为正时，从左向右取值。步长为负时，反向取值，默认为1，步长值不能为0

```python
List = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
print(List[0:])
print(List[:])
print(List[2:])
print(List[2:4])
print(List[2:100])

print('============================')
print(List[:-1])
print(List[::-1])   
print(List[::-2])
```

这里的[start_index:end_index] 是一个前开后闭的区间取值，例如[2:4]实际数学表示是[2:4)，也就是获取第3个和第4个值。 

运行结果如下

```python
[1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
[1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
[3, 4, 5, 6, 7, 8, 9, 10]
[3, 4]
[3, 4, 5, 6, 7, 8, 9, 10]
============================
[1, 2, 3, 4, 5, 6, 7, 8, 9]
[10, 9, 8, 7, 6, 5, 4, 3, 2, 1]
[10, 8, 6, 4, 2]
```

注意：切片操作不会出现索引(下标)越界的情况，Python内部有处理机制自动转换了，这里和通过索引访问列表list是不一样的。

### 八、列表的常用方法总结

用append()方法，在列表尾部添加单个新元素。 
+  用insert()方法，在列表中指定位置添加元素。 
+  用 “+” 运算符，将两个列表拼接出一个新列表。 
+  用extend()方法，在一个列表后面拼接进另一个列表。 
+  用del list[m] 语句，删除指定索引m处的元素。 
+  用remove()方法，删除指定值的元素（第一个匹配项）。 
+  用pop()方法，取出并删除列表末尾的单个元素。 
+  用pop(m)方法，取出并删除索引值为m的元素。 
+  用clear()方法，清空列表的元素。（杯子还在，水倒空了） 
+  用del list 语句，销毁整个列表。（杯子和水都没有了） 
+  用len()方法，统计全部元素的个数。 
+  用count()方法，统计指定值的元素的个数。 
+  用max()方法，统计元素中的最大值（要求元素类型相同；数字类型直接比较，其它类型比较id） 
+  用min()方法，统计元素中的最小值（要求元素类型相同；数字类型直接比较，其它类型比较id） 
+  用index()方法，查找指定值的元素的索引位置（第一个匹配项）。 
+  用reverse()方法，翻转列表中的元素。 
+  用copy()方法，浅拷贝并生成新的列表。 
+  用deepcopy()方法，深拷贝并生成新的列表。 
+  用sort()方法，在原列表基础上进行排序。 
+  用sorted()方法，将新列表基础上对原列表的元素进行排序。

### 九、扩展习题

切片操作()

```python
#判断回文
s = 'abba'
if s == s[::-1]:
    print("True")
else:
    print("False")

#数字翻转
#对于一个整数X，定义操作rev(X)为将X按数位翻转过来，并且去除掉前导0
def rev(x):
    x = str(x)
    return int(x[::-1])

z = 10590
print(rev(z))
```

来点难度，leetcode题目，

```python
Given an array of size n, find the majority element. The majority element is the element that appears more than ⌊ n/2 ⌋ times.

You may assume that the array is non-empty and the majority element always exist in the array.

Example 1:

Input: [3,2,3]
Output: 3
Example 2:

Input: [2,2,1,1,1,2,2]
Output: 2
```

最好理解的解法

```python
class Solution:
    def majorityElement(self, nums):
        majority_count = len(nums)//2
        for num in nums:
            count = sum(1 for elem in nums if elem == num)
            if count > majority_count:
                return num
#遍历数组，某元素出现次数大于半数，就返回该元素
#注意这里用到了生成器(1 for elem in nums if elem == num)
```

最快的解法

```python
class Solution:
    def majorityElement(self, nums):
        nums.sort()
        return nums[len(nums)//2]
#众数必然出现在中间位置
```

### 常见的错误

+ TypeError: ‘list’ object is not callable 
+ 命名错误，使用了保留字list定义了变量

```python
list = [1, 2, 3] #list是保留字，不能做变量
my_list = ['Google', 'Yahoo', 'Baidu', 'Sogou', 'Google']
newlist = list(set(my_list))
print(newlist)
```

### 参考资料

[https://mp.weixin.qq.com/s/OKf7N3xxrS0tJ_K8Srw-jA](https://mp.weixin.qq.com/s/OKf7N3xxrS0tJ_K8Srw-jA) 
+ 
[https://www.geeksforgeeks.org/python-list/](https://www.geeksforgeeks.org/python-list/) 
+ 
[https://www.yuanrenxue.com/python/python-list.html](https://www.yuanrenxue.com/python/python-list.html)

转载请注明：[IPCPU-网络之路](https://www.ipcpu.com) » [Python的list列表详解](https://www.ipcpu.com/2021/07/python%e7%9a%84list%e5%88%97%e8%a1%a8%e8%af%a6%e8%a7%a3/) 

### 参考文章
[Python的list列表详解](https://www.ipcpu.com/2021/07/python%e7%9a%84list%e5%88%97%e8%a1%a8%e8%af%a6%e8%a7%a3/)