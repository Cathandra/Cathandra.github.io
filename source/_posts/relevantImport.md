---
title: Python相对导入
date: 2019-11-16 15:33:02
tags: 
    - python
categories: [Back-End]
---

对于结构复杂的项目，相对导入是有必要的。对于一个项目，很可能会产生文件或包更名的情况，相对导入就真好为之提供了代码维护的便捷。  
P.S.这次是因为接手别人的代码发现的问题。  
正好就补一补这个盲区了~

## 基本语法

>
>绝对导入的格式为 `import A.B` 或 `from A import B`  
>相对导入格式为 `from . import B` 或 `from ..A import B`，`.`代表当前模块，`..`代表上层模块，`...`代表上上层模块，依次类推。
>

<!-- More -->

## 基本规则

1. 如果是绝对导入，一个模块只能导入自身的子模块或和它的顶层模块同级别的模块及其子模块。

2. 如果是相对导入，一个模块必须有**包结构**、且只能导入它的**顶层模块**内部的模块。

    2.1. 关于**包结构**：文件夹被python解释器视作package需要满足两个条件：  
    - 文件夹中必须有`__init__.py`文件，该文件可以为空，但必须存在该文件；  
    - 不能作为顶层模块来执行该文件夹中的py文件（即不能作为主函数的入口）。

    2.2. 关于**顶层模块**：如果一个模块被直接运行，则它自己为顶层模块。  

记住这2条，大部分问题就可以解决了。

## 举个例子

假设文件系统上有mypackage包，组织如下：

```file
mypackage/
    __init__.py
    main.py
    A/
        __init__.py
        spam.py
        grok.py
    B/
        __init__.py
        bar.py
```

### 包内引入

以运行`mypackage`下的`main.py`为前提，如果在package`A`的`spam.py`中导入`grok.py`，使用显式相对导入(explicit relative import)则应：

```python
from . import grok
```

### 跨包引入

同样前提，如果在package`A`的`spam.py`中导入package`B`的bar.py`，则应：

```python
from ..B import bar
```

### 向上一级引入

以运行package`A`下的`spam.py`为前提，在`spam.py`中导入`mypackage`下的`main.py`，（这就是我接手时面对的情况），则应：~~直接面对报错~~

```code
ImportError: attempted relative import with no known parent package
```

理由见基本规则2。  
此时应修改项目结构。

## 扩展阅读

到这里，算是把方法简单归纳了一遍。至于具体的机理，还没有深入探讨。  
鉴于最近的身体状况，暂时就把资料贴在这里，把这项任务放进TODO LIST中了。

1. [stackoverflow - Relative imports for the billionth time](https://stackoverflow.com/questions/14132789/relative-imports-for-the-billionth-time#answer-14132912)  
及其翻译  
[Python相对导入机制详解](https://laike9m.com/blog/pythonxiang-dui-dao-ru-ji-zhi-xiang-jie,60/)

## 参考资料

1. [python3-cookbook » 第十章：模块与包 » 10.3 使用相对路径名导入包中子模块](https://python3-cookbook.readthedocs.io/zh_CN/latest/c10/p03_import_submodules_by_relative_names.html)  
2. [Python 文档 » 5.7. 包相对导入](https://docs.python.org/zh-cn/3/reference/import.html#package-relative-imports)  
3. [Huoty's Blog - Python 相对导入与绝对导入](http://kuanghy.github.io/2016/07/21/python-import-relative-and-absolute)  
4. [青山牧云人 - Python包的相对导入时出现错误的解决方法](https://www.cnblogs.com/ArsenalfanInECNU/p/5346751.html)  
