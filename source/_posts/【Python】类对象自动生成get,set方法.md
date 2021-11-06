---
title: 【Python】类对象自动生成get,set方法
author: Will Holmes
categories: Python
tags:
  - Python
  - 面向对象
date: 2021-11-07 03:42:24
---


代码

```python

    class Student:
        def __init__(self):
            self.name = None
            self.age = None
    
    if __name__ == '__main__':
        student = Student()
        print(student.__dict__)
        for k in student.__dict__:
            print("def set_" + k + "(self," + k + "):")
            print("\tself." + k, "=" + k)
            print("def get_" + k + "(self):")
            print("\treturn self." + k)
    

```
输出结果：

```python

    {'name': None, 'age': None}
    def set_name(self,name):
    	self.name =name
    def get_name(self):
    	return self.name
    def set_age(self,age):
    	self.age =age
    def get_age(self):
    	return self.age
    

```
将get,set方法赋值粘贴到类方法

**将字典转化为对象**

```python

    def dict_to_obj(dictObject: dict, obj):
        for k,v in dictObject.items():
            obj.__dict__[k] = v
        return obj
    
    
    if __name__ == '__main__':
        d = {"id":"123", "name":"class", "age":"18"}
        stu: Student = dict_to_obj(d, Student())
        print(stu.id)
        print(stu.age)
        print(stu.name)
        print(stu.get_id())
        print(stu.get_name())
        print(stu.get_age())
    

```
工具类

```python

    def dict_to_str(dict_object: dict, ensure_ascii=False):
        return json.dumps(dict_object, ensure_ascii=ensure_ascii)
    
    def str_to_dict(str_object: str):
        dic = json.loads(str_object)
        return dic
    
    def object_to_json_string(self, ensure_ascii=False):
        return json.dumps(self, default=lambda o: o.__dict__, ensure_ascii=ensure_ascii)
    
    

```
