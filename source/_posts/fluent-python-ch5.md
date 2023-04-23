---
layout: post
title: "Fluent Python - 一等函数"
date: 2023-03-21 17:52:16
tags:
	- python
categories: [Back-End]
---

***

《流畅的Python》第五章一等函数读书笔记。

## 快速回忆

1.  什么是“一等对象”？
2.  什么是“函数式风格编程”？什么是“高阶函数”？有哪些内置函数是高阶函数请举例？

\*函数`all()`、`any()`用法

1.  创建函数对象的方式？有哪些可调用对象？
2.  怎样输出一个对象的属性？
3.  有哪几种常见的函数传参方式？可变关键字参数如何标识？传入关键字参数后，函数是否还接收位置参数？

\*“方法”是什么

## 一等对象

一等对象`first-class object`指满足以下条件的程序实体：（运行时创建、能赋值、能传参、能返回）

-   在运行时创建
-   能赋值给变量或数据结构中的元素
-   能作为参数传给函数
-   能作为函数的返回结果

Python中，整数、字符串、字典和所有的函数都是一等对象 Python中，整数、字符串、字典和所有的函数都是一等对象 *。* Python函数是对象，其type是function类的实例。 Python函数是对象，其type是function类的实例。

e.g.0 函数对象的属性`__doc__`可用于生成帮助文本：

```python
def factorial(n):
  """renturns n!"""
  return 1 if n <2 else n * factorial(n-1)

factorial.__doc__  # 返回'renturns n!'
```

e.g.1 通过别名使用函数

```python
fact = factorial
fact(5)  # 返回120
```

e.g.2 把函数作为参数传递

```python
map(factorial, range(11))  # <map object at 0x...>
list( map(factorial, range(6)) )  # [1, 1, 2, 6, 24, 120, 720]
```

有了函数作为一等对象的概念后，就可以使用**函数式风格编程**，其中一个特点就是使用高阶函数。

## 高阶函数

接受函数为参数，或把函数作为结果返回的函数是高阶函数（higher-order function）。

e.g. 内置函数`sorted`的可选参数key用于提供一个函数应用到各个元素上排序。

```python
fruits = ['strawberry', 'fig', 'apple', 'cherry']
sorted(fruits, key=len)  # 按单词长度给列表排序，结果为 ['fig','apple','cherry','strawberry']
```

```python
def reverse(word):
  return word[::-1]
sorted(fruits,key=reverse)
```

## 匿名函数

lambda关键字，不多做赘述。

## 可调用对象\*

除了用户定义的函数，调用运算符`()`还可以应用到其他对象上，使用内置的`callable()`函数可以判断对象是否可调用。

Python的7种可调用对象包括：

-   用户定义的函数
-   内置函数：使用C语言实现的函数，如`len`或`time.striftime`
-   内置方法：使用C语言实现的方法，如`dict.get`
-   方法：在类的定义体中定义的函数
-   类：调用类时会运行类的\_\_new\_\_方法创建一个实例，然后运行\_\_init\_\_方法，初始化实例，最后把实例返回给调用方。因为Python没有new运算符，所以调用类相当于调用函数。（通常，调用类会创建那个类的实例，不过覆盖\_\_new\_\_方法的话，也可能出现其他行为。）
-   类的实例：如果类定义了\_\_call\_\_方法，那么它的实例可以作为函数调用。
-   生成器函数：使用yield关键字的函数或方法。调用生成器函数返回的是生成器对象。生成器函数在很多方面与其他可调用对象不同，详情参见第14章。生成器函数还可以作为协程，参见第16章。

## 用户定义的可调用类型

任何Python对象都可以表现得像函数（再次强调，函数是一种特殊的对象）。实例方法`__call__`的类型是一个method-wrapper，作为可调用对象协议，作用是实现`()`运算符，将实例作为函数调用。

```python
import random
class BingoCage:
def __init__(self, items):
    self._items = list(items)  
    random.shuffle(self._items)  
def pick(self):  
    try:
        return self._items.pop()
    except IndexError:
        raise LookupError('pick from empty BingoCage')  
def __call__(self):  # 因为实现了__call__方法，bingo可以被调用；bingo.pick()的快捷方式是bingo()
    return self.pick()
```

## 函数内省

函数内省是指在运行时获取函数的信息和属性，来更好地理解和使用函数。

使用`dir()`函数可以输出对象的属性；\_\_code\_\_属性（及他自身的属性co\_varnames给出参数名称、co\_argcount参数个数），\_\_defaults\_\_属性给出参数默认值。

inspect module提供了inspect.signature()获取签名、inspect.getsource()源码和inspect.getclosurevars()闭包的方法。

## 位置参数和仅限关键字参数

Python的特性之一是提供了极为灵活的参数处理机制。在调用函数时，使用*和* \*“展开”可迭代对象，映射到单个参数。

```python
def func(a=None, b=None, *args, **kwargs):
    print(a, b, args, kwargs)

# 1. 位置参数（positional arguments）：按 位置顺序 传参
func(1, 2)
# 1 2 () {}


# 2. 关键字参数（keyword arguments）：按 关键字=值 传参
func(a = 1, b = 2)
# 1 2 () {}


# 3. 可变参数
func(1, 2, 3, 4)
# 1 2 (3, 4) {}
func(1, 2, 3, 4, {"c": 5, "d":6})
# 1 2 (3, 4, {'c': 5, 'd': 6}) {}   参数里的字典会作为位置参数传入
# func(a = 1, b = 2, 3, 4)  # 关键字参数后不能跟位置参数


# 4. 可变关键字参数
func(1, b = 2, c = 3, d = 4)
# 1 2 () {'c': 3, 'd': 4}
func(1, 2, 3, 4, **{"c": 5, "d":6})
# 1 2 (3, 4) {'c': 5, 'd': 6}     注意：** 可以解包字典，使之成为 c=5,d=6
func(1, b = 2, c = 3, d = 4, e = {"c": 5, "d":6})
# 1 2 () {'c': 3, 'd': 4, 'e': {'c': 5, 'd': 6}}

```

-   **位置参数**：位置参数按位置顺序传参
-   **关键字参数**：通过关键字的方式传参，可以使用 **关键字=值** 的得方式传参，默认参数是在函数定义时就给参数传入了一个默认的参数值，如果函数调用时没有给这个参数传值，就使用默认值， 如果显式的传参了，就使用新传入的值代替默认值。
-   可变参数：可变参数是一个形参可以接受多个实参，用`*args `标志这是一个可变参数。可变参数的传入数是不确定的，通常由函数调用方决定。可变参数会将所有接受的值以元组的形式传入函数体内，如果可变参数没有接受到任何值，则传入一个空元组`(, )`。
-   可变关键字参数：可变关键字参数 用 双星号+参数名表示, 如：`** kwargs`，可变关键字参数接收零个或多个关键字参数，并以字典的形式传入函数体，关键字为此字典的key，关键字绑定的值为value。如果可变关键字没有接收到任何参数， 则传入函数体一个空字典`{}`。
-   仅限关键字参数：仅限关键字参数不可缺省（除非有默认值），且只能强制性通过关键字传参。可变参数后面的关键字参数都是仅限关键字参数。单星号`*`表示不接受任何可变参数，它可以作为普通参数的结束标志。
-   仅限位置参数：仅限位置参数只能通过位置传参，不能使用关键字传参(a=1)。这是 Python3.8 新特性之一，在函数定义时，参数之间可指定一个**斜杠（****`/`****）**，斜杠前的参数严格遵守仅位置参数的定义。

## 函数注解

注解为函数生命中的参数和返回值增加元数据。

参数的注解表达式可以用`:`添加在参数后，如果参数有默认值，注解放在参数名和=之间，注解的常见类型为：类（如str）或字符串（如'int > 0'）；返回值的注解用`->`添加在函数声明语句末尾。

注解不做任何处理，本质上是存储在函数的`__annotation__`属性中，不做检查、强制和验证。

```python
def clip(text:str, max_len:'int > 0'=80)-> str:  # 有注解的函数声明 
    """在max_len前面或后面的第一个空格处截断文本
    """
    end = None
    if len(text) > max_len:
        space_before = text.rfind(' ', 0, max_len)
        if space_before >= 0:
            end = space_before
        else:
            space_after = text.rfind(' ', max_len)
            if space_after >= 0:
                end = space_after
    if end is None:  # 没空格
        end = len(text)
    return text[:end].rstrip()

print(clip.__annotations__)  #本质上是存储在函数的annotation属性中 
# {'text': <class 'str'>, 'max_len': 'int > 0', 'return': <class 'str'>} 
```

## 参考资料

1.  [What are "first-class" objects?](https://stackoverflow.com/questions/245192/what-are-first-class-objects "What are \"first-class\" objects?")
2.  [木易可 - 流畅的Python](https://www.bilibili.com/video/BV18i4y1u7Cr?p=18 "木易可 - 流畅的Python")
3.  [\[Python\] 仅限位置参数和仅限关键字参数](https://blog.csdn.net/Spade_/article/details/107751813 "\[Python] 仅限位置参数和仅限关键字参数")
