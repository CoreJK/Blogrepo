---
title: "Python魔术方法之『__dict__』"
date: 2022-04-08T13:05:19+08:00
description: "获取对象的所有属性，并以字典形式返回"
draft: false
hideToc: false
enableToc: true
enableTocContent: false
tocPosition: inner
tocLevels: ["h2", "h3", "h4"]
tags:
- "python"
categories:
- "编程语言"
series:
- "技术研究"
image:
---


在这篇文章中，我们一起看一下Python提供的内置魔法方法`__dict__`
你将会明白到什么是 `__dict__` ，以及如何在代码中使用它，它与`dir()`函数有什么不同

## ‘*\_\_dict\_\_*’ 在 Python 中的角色?

Python 使用一个特殊的内置属性 `__dict__` 来存储对象的可变属性

通过`object.__dict__`可以得到一个字典，包含着属性的『键/值』对

`__dict__` 属性不仅限于实例对象，也可以用于用户定义的函数、模块、类 （除非声明了 `__slot__`）

看看下面的例子：

```python
class Employee:
	def __init__(self, name, age):
		self.name = name
		self.age = age

employee1 = Employee('John', 23)
employee2 = Employee('Rambo', 28)

print("The output of employee1.__dict__ is : ", employee1.__dict__)
print("The output of employee2.__dict__ is : ", employee2.__dict__) 
```

`__dict__ `分别把每一个 `employeel` 对象的属性，以字典的形式返回了

```
The output of employee1.dict is : {'name': 'John', 'age': 23}
The output of employee2.dict is : {'name': 'Rambo', 'age': 28}
```

如果类本身，没有任何属性，也不要求实例化过程中，绑定属性值
```python
class A:
	pass

a = A()
print(a.__dict__) 
>>> {}
```

对象 `a.__dict__`，会返回一个空字典

## 如何使用\_\_*dict*\_\_来检索属性值?

`__dict__`返回的内容是字典，因此我们可以使用字典获取键值的常规方法，来获得对象的任何属性值。

```python
class A:
	pass

a = A()
print(type(a.__dict__))

Output:
>>> <class 'dict'>
```

为了获得 `person` 对象的`name` 属性的值，可以按照下面的例子获取到

```python
class Person:
	def __init__(self, name):
		self.name = name

person = Person('John') 
person.__dict__['name'] 

Output:
>>> John
```

## 如何使用\_\_*dict*\_\_来设置属性值?

我们也可以直接通过 `__dict__` 来给对象的属性赋值

```python
class A:
	def __init__(self, x):
		self.x = x

a = A(1)
print(a.x)
>>>1

a.__dict__['x'] = 2
print(a.x)
>>> 2
```

## 如何使用\_\_*dict*\_\_来添加新的属性?

既然可以修改，也能够添加一个不存在的属性值

```python
class A:
	def __init__(self, x):
		self.x = x

a = A(1)
a.__dict__['y'] = 2

print(a.y) 
>>> 2
```

让我们验证一下，刚刚增添的属性 `y` 是否正确的和对象 `a` 绑定在了一起：

```python
print(a.__dict__) 
>>> {'y': 2, 'x': 1}
```

## 如何使用\_\_*dict*\_\_来删除属性？?

当我们想要删除某个属性，通过 `del` 就可以了：

```python
class A:
	def __init__(self, x, y):
		self.x = x
		self.y = y

a = A(1, 2)
print("before deleting:")
print(a.__dict__)

del a.__dict__['y']
print("after deleting:")
print(a.__dict__) 

Output:
before deleting:
{'y': 2, 'x': 1}
after deleting:
{'x': 1}
```

## 函数调用\_\_dcit\_\_

The \_\_*dict*\_\_ attribute is also available to be used for normal functions(declared using ‘def’) and lambda. 

`__dict__` 方法也可以作用于普通的函数、`lambda` 匿名函数

```python
def foobar():
	pass

print(foobar.__dict__) 
>>> {}
```

The \_\_dict\_\_, in this case, contains the [function attributes](https://www.python.org/dev/peps/pep-0232/).

So, we can do something like this

```python
foobar.x = 1
foobar.__dict__
>>> {'x': 1}
```

我们也可通过 `__dict__` 访问函数的『属性』`x`的值

```python
foobar.__dict__['x']
>>> 1
```

## 类调用\_\_dcit\_\_

We can also access class methods and class variables using the \_\_*dict*\_\_ attribute.

我们也可以通过 `__dict__` 属性，直接访问类本身的方法和类属性

```python
class Foobar:
	pass

print(Foobar.__dict__) 
```

Output:

```python
{'__dict__': <attribute '__dict__' of 'Foobar' objects>,
              '__doc__': None,
              '__module__': '__main__',
              '__weakref__': <attribute '__weakref__' of 'Foobar' objects>}
```

在 `Foobar` 中，定义类属性`class_var`，和一个类方法`cls_method`

```python
class Foobar:
	class_var = 1
	@classmethod
	def cls_method(cls, name):
		pass

print(Foobar.__dict__) 
```

Output:

```python
{'__dict__': <attribute '__dict__' of 'Foobar' objects>,
              '__doc__': None,
              '__module__': '__main__',
              '__weakref__': <attribute '__weakref__' of 'Foobar' objects>,
              'class_var': 1,
              'cls_method': <classmethod object at 0x000002815DCCBA30>}
```

## 模块调用\_\_dcit\_\_

任何时候，例如：如果我们`A.py`文件中，把 `B`文件作为模块导入

然后访问 B 文件中的一个变量的值，像这样：

```
# 假设这是 A.py 文件，B.py 文件中含有一个变量 x = None
import B
B.x = 1
```

实际上，python 悄悄地调用(隐式调用)了 `B.__dict__[x]=1`，来实现对模块 `B` 中变量 `x` 的赋值操作

## \_\_dict\_\_ & dir 的区别

`dir()` 函数可以列出对象、对象可用所属的类、父类的所有属性和方法

```python
class Fruit:
	cls_var = 1

	def __init__(self, name):
		self.name = name

	def something(self):
		pass

f = Fruit('Apple')
print("n dir(f) contains:")
print(dir(f))
print("n f.__dict__ contains:")
print(f.__dict__) 
```

Output:

```python
>>> dir(f) contains:
>>>
['__class__','__delattr__',
 '__dict__','__dir__','__doc__',
 '__eq__', '__format__',
 '__ge__','__getattribute__',
 '__gt__','__hash__',
 '__init__','__init_subclass__',
 '__le__','__lt__',
 '__module__','__ne__',
 '__new__','__reduce__',
 '__reduce_ex__','__repr__',
 '__setattr__','__sizeof__',
 '__str__','__subclasshook__',
 '__weakref__','cls_var','name',]
```

`__dict__` 只包含对象的局部属性

```python
>>> f.__dict__ contains:
>>> {'name': 'Apple'}
```

如果要获取类的变量值，通过 `类名.__dict__`就可获取到了

```python
print(Fruit.__dict__) 
```

Output:

```python
{'__dict__': <attribute '__dict__' of 'Fruit' objects>,
              '__doc__': None,
              '__init__': <function Fruit.__init__ at 0x000002815DC70310>,
              '__module__': '__main__',
              '__weakref__': <attribute '__weakref__' of 'Fruit' objects>,
              'cls_var': 1,
              'something': <function Fruit.something at 0x000002815E0AFDC0>}
```

相较于 dir() 只能访问，`__dict__`能够做到修改和删除对象的属性值

```python
print(f.__dict__)
f.__dict__['name'] = 'Orange'
print(f.__dict__) 

>>> {'name': 'Apple'}
>>> {'name': 'Orange'}

del f.__dict__['name']
print(f.__dict__) 

>>> {}
```

`__dict__`并不能访问一切对象的属性，例如：

1. 内建的数据类型
2. 对象的类中的属性，被赋值给了 `__slots__` 变量

```python
>>> a = [1,2]
>>> a.__dict__
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'list' object has no attribute '__dict__' 
```

```python
>>> dir(a)
['__add__','__class__','__contains__',
 '__delattr__','__delitem__',
 '__dir__','__doc__','__eq__',
 '__format__','__ge__','__getattribute__',
 '__getitem__','__gt__','__hash__',
 '__iadd__','__imul__','__init__',
 '__init_subclass__','__iter__','__le__',
 '__len__','__lt__','__mul__','__ne__','__new__',
 '__reduce__','__reduce_ex__',
 '__repr__','__reversed__','__rmul__',
 '__setattr__','__setitem__','__sizeof__',
 '__str__','__subclasshook__',
 'append','clear','copy','count',
 'extend','index','insert','pop','remove','reverse',
 'sort']
```

**实例的属性值，被赋值给了 `__slots__`变量**

```python
class Person:
	__slots__ = ['name', 'age']

	def __init__(self, name, age):
		self.name = name
		self.age = age

p = Person('Jason', 34)
p.__dict__ 
```

Output:

```python
Traceback (most recent call last):
  File "person.py", line 11, in <module>
    p.__dict__
AttributeError: 'Person' object has no attribute '__dict__
```

Output:

```python
print(dir(p)) 
['__class__', '__delattr__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__le__', '__lt__', '__module__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__slots__', '__str__', '__subclasshook__', 'age', 'name']
```

## 参考

尝试翻译的这篇英语原文，提升自己的英语阅读能力XD

1. [Understanding ‘__dict__’ in Python](https://arrayjson.com/__dict__-python/)
