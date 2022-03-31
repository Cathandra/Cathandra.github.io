---
layout: post
title: "Fluent Python - 序列构成的数组"
date: 2021-12-17 09:03:16
tags:
	- python
categories: [Back-End]
---

《流畅的Python》第二章序列构成的数组读书笔记。

本章主要目标是深入理解不同序列类型，避免重复造轮子、帮助开发者将自己定义的API和原生序列保持一致。

## 快速回忆

1. 内置序列类型可以按哪些标准分成哪几类、并举例？

	-直接赋值、深、浅拷贝的区别？可变与不可变对象作为函数参数传递的区别？

2. 生成器表达式是什么？如何创建？其优点是什么？

	-字典和集合是否有推导机制？

3. 元组的创建方式是什么（只有一个元素的元组）？元组拆包有哪些形式？拆包时使用的占位符有哪些、其区别是什么？如何进行压包操作？

4. 如何定义和使用具名元组？

5. `slice()`的作用是什么？使用切片如何对序列进行嫁接、切除或匹配修改？

	如何进行多维切片？

6. list.sort方法和内置函数sorted的区别？

7. `bisect()`和`insort()`的作用？

8. array数组有哪些优势？如何进行排序操作？

   -方法`map()`、`filter()`、`enumerate()`、`divmod()`、`slice()`、`ord()`的作用？

<!-- More -->

## 内置序列类型

### 按存放的数据类型区分

#### 容器序列

存放对象的引用，可以存放任意类型

举例：`list` `tuple` `collections.deque`

#### 扁平序列

存放对象的值，因此实质是一段连续的内存空间，只能存放基础类型（字符、数值等）

举例：`str` `bytes` `bytearray` `memoryview` `array.array`

### 按能否被修改区分

#### 可变序列

指变量值发生改变后内存地址不变的序列

举例：`list` `bytearray` `array.array` `collections.deque` `memoryview`

```python
lst = ['a']
print(id(lst))  # 2459767072904
lst.append('b')
print(id(lst))  # 2459767072904 修改可变序列后地址不发生变化
```

#### 不可变序列

举例：`tuple` `str` `bytes`

```python
s = 'a'
print(id(s))  # 2459698678640
s += 'b'
print(id(s))  # 2459788551216 修改不可变序列后地址发生变化
```

#### -从可变与不可变引申开去

引用某位秀儿同学的话：和“浅”、“引用”、“地址”相关的都是指向一个reference的。

##### 值引用和地址引用

>- **b=a**：赋值引用，a和b指向同一对象
>
>	![](b=a.png)
>
>- **b = a.copy():** 浅拷贝, a 和 b 是一个独立的对象，但他们的子对象还是指向统一对象（是引用）
>
>	![](b=a.copy().png)
>
>- **b = copy.deepcopy(a):** 深度拷贝, a 和 b 完全拷贝了父对象及其子对象，两者是完全独立的[[7](#link7)]
>
>	![](b=copy.deepcopy(a).png)
>	
>	```python
>	import copy
>	a = [1, 2, 3, 4, ['a', 'b']] # 原始对象
>	 										
>	b = a                       # 赋值，传对象的引用
>	c = copy.copy(a)            # 对象拷贝，浅拷贝
>	d = copy.deepcopy(a)        # 对象拷贝，深拷贝
>	 										
>	a.append(5)                 # 修改对象a
>	a[4].append('c')            # 修改对象a中的['a', 'b']数组对象
>	 										
>	print( 'a = ', a )
>	print( 'b = ', b )
>	print( 'c = ', c )
>	print( 'd = ', d )
>	# ('a = ', [1, 2, 3, 4, ['a', 'b', 'c'], 5])
>	# ('b = ', [1, 2, 3, 4, ['a', 'b', 'c'], 5])
>	# ('c = ', [1, 2, 3, 4, ['a', 'b', 'c']])
>	# ('d = ', [1, 2, 3, 4, ['a', 'b']])
>	```
>	
##### 参数传递

一般的，有如下规律：

1. 不可变对象作为函数参数，相当于值传递；可变对象作为函数参数，相当于引用传递。[[8](#link5)]

2. 一旦函数中遇到=，就会重新开辟一个内存空间，不再对传递过来的参数有影响。

	```python
	def change_method(param):
		param.append("abc")
		param = ["a","b","c"]  # 实质上不对参数有影响
	
	lst = ["1", "2"]
	change_method(lst)
	print(lst) # ["1", "2", "abc"]
	```

## 列表推导式和生成器表达式

### 列表推导式

列表推导式内部的变量只在局部起作用，不会产生变量泄露的问题。

-列表推导式实现的功能也可以用`filter`和`map`共同实现，但列表推导式可以增强代码的可读性与高效性。这一点在《Effective Python》第27条亦有说明。

```python
symbols = '#$%^&'
beyond_ascii = [ord(s) for s in symbols if ord(s) > 127]  # ord()方法返回字符串对应的Unicode值（其中ASCII部分的等于ASCII值），这段推导式的作用是找出字符串中ASCII > 127的字符
beyond_ascii = list(filter(lambda c: c > 127, map(ord, symbols)))  # 用filter和map实现列表推导式的功能
```

-`filter`是内置的过滤器函数，作用是对可迭代类型的元素进行判断，然后返回新列表。

-`map(函数名，可迭代类型)`作用是把可迭代类型的元素分别通过函数名加工，然后生成新列表。

列表推导式生成2个或以上可迭代对象的笛卡尔积。

```python
colors = ['black', 'white']
sizes = ['S', 'M', 'L']
tshits = [(color, size) for color in colors for size in sizes]  # 嵌套循环的顺序和for的先后顺序一样
```

除列表外，字典和集合也有推导机制:

- 字典推导式

```python
a = list(range(1,11))
even_squares_dict = {x:x**2 for x in a if x%2 == 0}  # {2: 4, 4: 16, 6: 36, 8: 64, 10: 100}
```

- 集合推导式

```python
a = list(range(1,11))
three_cubed_set = {x**3 for x in a if x%3 == 0}  # {27, 216, 729}
```

### 生成器表达式

生成器表达式的创建方法是把列表扩展式的`[]`换成`()`。它遵守了迭代器协议，可以逐个产出元素，而不是先创建列表再把列表传到构造函数里。与后者相比，生成器表达式的主要优点是可以**节省内存**。

## 元组

元组的定义是通过逗号分割。

元组存放了每个字段的位置和数据，其实是对数据的记录。

### 元组拆包

#### 平行赋值

平行赋值指的是：把一个可迭代对象的元素一并赋值到对应变量组成的元组中。

```python
lax_coordinates = (33, -118)
latitude, logitude = lax_coordinates 
```

除tuple以外，python中任何序列或可迭代对象（如：列表、元组、字符串、文件对象、迭代器和生成器等），都可通过类似这样的简单赋值语句拆包给多个变量。

- 列表

-《Effective Python》第7条：尽量用`enumerate`取代`range`。这是因为形如`range(len(lst))`的代码可读性差，

> `enumerate()`能够把任何一种迭代器封装成惰性生成器（lazy generator，参见第30条）。这样的话，每次循环的时候，它只需要从iterator里面获取下一个值就行了，同时还会给出本轮循环的序号，即生成器每次产生的一对输出值。[[3](#link3)]

```python
a,b,c = ['a', 'b', 'c']
a  # 'a'

a,b,c = enumerate(['a', 'b', 'c'])  # ((0, 'a'), (1, 'b'), (2, 'c'))
a  # (0, 'a')
```

- 字典
```python
a,b,c = {'a':1, 'b':2, 'c':3}
a  # 'a'

a,b,c = {'a':1, 'b':2, 'c':3}.items()
a  # ('a', 1)
```

- 字符串
```python
a,b,c = 'abc'
a  # 'a'
```

- 生成器
```python
a,b,c = (x for x in range(3))
a  # 0
```

**变量须跟序列元素的数量一致**，或需用 `*`和`_` **占位符**表示忽略多余的元素，否则会抛出ValueError异常。

```python
# _占位符
info = ["Alice", 20, 168, 55, (1999,10,22)]
_,age, height,_,_ = info
age  #  20
_  # (1999, 10, 22)

# *占位符：返回格式为list
a, b, *c = range(5)
a, b, c  # (0, 1, [2, 3, 4])
```

#### 不使用中间变量交换两变量的值

注：python在赋值语句中，总是在对变量进行实际设置之前，先对等号右侧进行全面评估；下面的例子中，会先将等号右侧打包成元组 `(a, b)` ，再顺序地赋值给等号左侧的 b, a 变量。

```python
b, a = a, b
```

#### 函数参数赋值

```python
t = (20, 8) 
divmod(*t)  # divmod(20, 8) ，divmod返回商和余数的元组

d = {'a':2, 'b': 1}
divmod(**d)  # divmod(2, 1)
```

#### 嵌套元组拆包

  ```python
  metro_areas = [
      ('Tokyo','JP',36.933,(35.689722,139.691667)),
      ('Delhi NCR', 'IN', 21.935, (28.613889, 77.208889)),
      ('Mexico City', 'MX', 20.142, (19.433333, -99.133333)),
      ('New York-Newark', 'US', 20.104, (40.808611, -74.020386)),
      ('Sao Paulo', 'BR', 19.649, (-23.547778, -46.635833))
  ]
  
  print('{:15} | {:^9} | {:^9}'.format(' ', 'lat.', 'long.'))  # :占位，^居中，.保留小数位数
  fmt = '{:15} | {:9.4f} | {:9.4f}'
  for name, cc, pop, (latitude, longitude) in metro_areas:  # 拆包
      if longitude <= 0:
          print(fmt.format(name, latitude, longitude))
  
  '''
                  |   lat.    |   long.  
  Mexico City     |   19.4333 |  -99.1333
  New York-Newark |   40.8086 |  -74.0204
  Sao Paulo       |  -23.5478 |  -46.6358
  '''
  ```

### 压包

压包是拆包的逆过程。

```python
a = ['a', 'b', 'c']
b = [1, 2, 3]
for i in zip(a, b):
    print(i)
# ('a', 1)
# ('b', 2)
# ('c', 3)
```

### 具名元组

`collections.namedtuple`用于构建一个带字段名的元组和一个有名字的类。使用`collections.namedtuple`构建的类的实例和元组相比占据的内存是一样的。

方法的第一个参数表示类的名称，第二个参数是类的字段名。后者可以是可迭代对象，也可以是空格隔开的字符串。

```python
from collections import namedtuple
ToDo = namedtuple('ToDo', 'days content priority')
t = ToDo(12, 'take a note', 1)
```

具名元组有类属性`_fields`、类方法`_make()`和实例方法`_asdict() `：

```python
# 1:类属性_fields
print(ToDo._fields)  # ('days', 'content', 'priority')

# 2:类方法_make(iterable) 
t1 = ToDo._make(t)
# 3:实例方法_asdict() 
print(t1._asdict())  # OrderedDict([('days', 12), ('content', 'take a note'), ('priority', 1)])
```

## 切片

### 规则

左闭右开、起止位置全部可见时可计算切片长度。好处是可以利用同样下标把序列分为不重叠的两部分：`lst[:2]` `lst[2:]`。

```python
invoice ='''
0.....6.................................40...........52.55........
1909  Pimoroni PiBrella                     $17.50     3    $52.50 
1489  6mm Tactile Switch x20                 $4.95     2    $9.90 
1510  Panavise Jr.-PV-201                   $28.00     1    $28.00 
1601  PitfT Mini Kit 320x240                $34.95     1    $34.95 
'''
SKU = slice(0, 6)
DESCRIPTION = slice(6, 40)
UNIT_PRICE = slice(40, 52)
QUANTITY = slice(52, 55)
ITEM_TOTAL = slice(55, None)

line_items = invoice.split('\n')[2:]
for item in line_items:
	print(item[UNIT_PRICE], item[DESCRIPTION])
'''
    $17.50   Pimoroni PiBrella                 
     $4.95   6mm Tactile Switch x20            
    $28.00   Panavise Jr.-PV-201               
    $34.95   PitfT Mini Kit 320x240      
'''
```

### 多维切片和省略

见[多维数组](#linkslice)

### 给切片赋值

把切片放在赋值语句左边，或作为`del`操作对象，可以对序列进行嫁接、切除或匹配修改。

```python
lst = list(range(10))
print(lst) # [0，1，2，3，4，5，6，7，8，9]
# 嫁接
lst[2:5] =[10, 30]
print(lst) # [0，1,10,30,5，6，7，8，9]
# 切除
del lst[5:7]
print(lst) # [0，1，10，30，5，8，9]
# 匹配修改
lst[2:5] = [100]
print(lst) # [0，1，100]
```



## 序列的增量赋值

增量赋值运算符`+=`和`*=`的表现取决于它们的第一个操作对象。 `+=`背后的特殊方法是\_\_*iadd*\_\_（就地加法）。但是如果类没有实现这个方法的话，Python会退一步调用*\_\_add\_\_*。 

```python
a = (1, 2, [3, 4])
a[2] += [6, 7]
'''Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'tuple' object does not support item assignment'''
a # (1, 2, [3, 4, 6, 7])
```

这个示例中，因为tuple不支持对他的元素赋值，所以会抛出TypeError异常。但是由于a[2]是个列表，本身是支持`+=`操作的，所以仍旧完成了操作，a的值变成(1, 2, [3, 4, 6, 7])。

- 不要把可变对象放在元组里面。
- 增量赋值不是一个原子操作。我们刚才看到它虽然抛出了异常，但还是完成了操作。

## list.sort方法和内置函数sorted

- list.sort方法就地排序列表；

- 内置函数sorted会新建一个列表作为返回值。

## 用bisect来管理已排序的序列

bisect模块包含两个主要方法，`bisect`和`insort`，两个方法都利用二分查找算法来在有序序列中查找或插入元素。

- bisect：在有序序列中查找某个元素的插入位置；
- insort：向有序序列中插入新元素，并保持排序。

## 数组、内存视图和队列

### 数组

当只需一个包含数字的列表时，数组比列表更高效。

- array.fromfile 读取速度快
- array.tofile 写入快
- 占用空间少

数组的排序：

```python
from array import array
from random import random, randrange
floats = array('d',(randrange(1, 50) for i in range(10)))  # d:双精度
print(floats)  # array(d,[7.0,34.0,15.0,16.0,30.0,5.0,2.0,44.0,14.0,19.0])
floats2 = array(floats.typecode, sorted(floats))
print(floats2)  # array('d',[2.0,5.0,7.0,14.0,15.0,16.0,19.0,30.0,34.0,44.0])
```

### 内存视图

```python
from array import array
from random import random

numbers = array('h', [-2,-1,0,1,2])
memv = memoryview(numbers)  # 5个短整型有符号整数的数组创建一个memoryview 
print(len(memv))  # 5
print(memv.tolist())  # [-2, -1, 0, 1, 2] 
memv_oct = memv.cast('B')  # 内存共享 转换成无符号字符类型 
print(memv_oct.tolist())  # [254, 255, 255, 255, 0, 0, 1, 0, 2, 0] TODO：涉及源码和补码

memv_oct[5] = 4
print(numbers)  # array('h', [-2, -1, 1024, 1, 2])
```

### Numpy数组

```python
import numpy

a = numpy.arange(1, 31)
print(type(a)) # <class 'numpy.ndarray'>
print(a.shape) # (30,)
a.shape = 6,5  # 把数组转换成二维
print(a)
'''
[[ 1  2  3  4  5]
 [ 6  7  8  9 10]
 [11 12 13 14 15]
 [16 17 18 19 20]
 [21 22 23 24 25]
 [26 27 28 29 30]]
 '''
print(a[2])  # [11 12 13 14 15]
print(a[2][1])  # 12
```
<span id='linkslice'>多维切片</span>

```python
print(a[6:2:-1,1:5:2])
'''
[[27 29]
 [22 24]
 [17 19]]
 '''
```

省略

```python
print(a[0:, ..., 0:3:2])
'''
[[ 1  3]
 [ 6  8]
 [11 13]
 [16 18]
 [21 23]
 [26 28]]
 '''
```

行列转换

```python
a.transpose()
'''
[[ 1,  6, 11, 16, 21, 26],
[ 2,  7, 12, 17, 22, 27],
[ 3,  8, 13, 18, 23, 28],
[ 4,  9, 14, 19, 24, 29],
[ 5, 10, 15, 20, 25, 30]]
'''
```

### 队列和双向队列

利用`append`和`pop`方法可以把列表作为栈或队列使用。

`collections.deque`双向队列则可以快速从两端添加或删除元素。

```python
from collections import deque
dq = deque(range(10), maxlen=10)
print(dq)  # deque([0, 1, 2, 3, 4, 5, 6, 7, 8, 9], maxlen=10)
dq.rotate(3)  # 逆时针旋转3
print(dq)  # deque([7, 8, 9, 0, 1, 2, 3, 4, 5, 6], maxlen=10)
dq.rotate(-4) 
print(dq)  # deque([1, 2, 3, 4, 5, 6, 7, 8, 9, 0], maxlen=10)
dq.appendleft(-1)
print(dq)  # deque([-1, 1, 2, 3, 4, 5, 6, 7, 8, 9], maxlen=10)
dq.extend([11,22,33])
print(dq)  # deque([3, 4, 5, 6, 7, 8, 9, 11, 22, 33], maxlen=10)
dq.extendleft([10,20,30,40])
print(dq)  # deque([40, 30, 20, 10, 3, 4, 5, 6, 7, 8], maxlen=10)
```

<center>![rotate(3)](rotate(3).png)</center>

## 参考资料

笔记和举例主要来自《流畅的Python》[[1](#link1)]，并结合了相关教程中的框架[[4](#link4)]。阅读过程中遇到不懂的内容有参考其他资料[[5-9](#link5)]。

1. [《流畅的Python》 - 第2章 序列构成的数组](https://weread.qq.com/web/reader/ab832620715c017eab864a6)<span id="link1"></span>
2. [《Python编程：从入门到实践（第2版）》 - 第3章: 列表简介 & 第4章: 操作列表](https://weread.qq.com/web/reader/08232ac0720befa90825d88kc20321001cc20ad4d76f5ae)
2. [《Effective Python：编写高质量Python代码的90个有效方法（原书第2版） 》 - 第2章：列表与字典 & 第4章：推导与生成](https://weread.qq.com/web/reader/c2932f9072620d81c29c1edkaab325601eaab3238922e53)<span id="link3"></span>
3. [木易可 - 流畅的Python](https://www.bilibili.com/video/BV18i4y1u7Cr)<span id="link4"></span>
4. [zlinzju - 元组拆包](https://blog.csdn.net/weixin_43026262/article/details/105449675)<span id="link5"></span>
5. [菜鸟教程 - Python enumerate() 函数](https://www.runoob.com/python/python-func-enumerate.html)
6. [菜鸟教程 - Python 直接赋值、浅拷贝和深度拷贝解析](https://www.runoob.com/w3cnote/python-understanding-dict-copy-shallow-or-deep.html)<span id="link7"></span>
6. [卓尔不群的雅典 - Python函数参数传递机制](https://www.jianshu.com/p/1eeca1a39243)
6. [可口可乐嗨 - python-变量与参数传递 ](https://www.cnblogs.com/marton/p/10668003.html)

## TODOs

本部分内容非常丰富，遗留了一些没有深入学习的部分：

1. 进一步理解：值传递和参数传递、深浅拷贝；
2. 元组拆包的`*`占位符；
3. 具名元组的有类属性、类方法和实例方法；
4. bisect模块；
5. 内存视图和队列。
