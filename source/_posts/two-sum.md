---
title: "两数之和"
date: 2021-01-08 10:23:54
tags:    - N3:LeetCode
categories: [Algorithm]
---

## 题目

[1. 两数之和](https://leetcode-cn.com/problems/two-sum/)

>给定一个整数数组 nums 和一个整数目标值 target，请你在该数组中找出 和为目标值 的那 两个 整数，并返回它们的数组下标。
>
>你可以假设每种输入只会对应一个答案。但是，数组中同一个元素不能使用两遍。
>
>你可以按任意顺序返回答案。
>
>**示例 1：**
>
>```
>输入：nums = [2,7,11,15], target = 9
>输出：[0,1]
>解释：因为 nums[0] + nums[1] == 9 ，返回 [0, 1] 。
>```
>
>**示例 2：**
>
>```
>输入：nums = [3,2,4], target = 6
>输出：[1,2]
>```
>
>**示例 3：**
>
>```
>输入：nums = [3,3], target = 6
>输出：[0,1]
>```
>

<!-- More -->

## 分析

我的直觉是类似暴力求解的方法：首先遍历数组，然后在当前值之后的每个值中寻找（目标值和当前值的差）。

这里就涉及一个关键的问题，即如果寻找到了差值，如何输出该值的数组下标。

答案是使用**哈希表**。

## Python实现

先提供一个仍然不脱离直觉的直观解决方法：

- 首先定义一个字典
- 将数组中的值作为字典的键，将下标作为字典的值存储
- 遍历数组，寻找加数，然后输出加数在字典中的值（即加数在数组的下标）

这种方法需要注意的点包括：

1. 在第三步时不能遍历hashmap，否则对于`nums = {3,3},target = 6`的情况下，hashmap的键会产生冲突；
2. 一堆下标和键值，**可读性真的很差**。

```python
def twoSum(self, nums: List[int], target: int) -> List[int]:
    hashmap = {}
    for i in range(len(nums)):
        hashmap[nums[i]] = i
    for i in range(len(nums)):
        if target-nums[i] in nums[i+1:]:
            return [i, hasmap.get(target-nums[i])]
```

更简洁优雅的方法：

- 使用`enumerate`方法，同时对下标和nums数组进行循环；
- 在循环的过程中，判断另一个加数似乎否在hashmap中，如果找到对应的加数，则直接返回；如果不在hashmap中，则将当前值插入hashmap。

这种方法需要注意的点包括：

1. 在获取hashmap的值时，最好使用`get()`而不是直接访问key。

```python
def twoSum(self, nums: List[int], target: int) -> List[int]:
    hashmap = {}
    for i, num in enumerate(nums):
        if (target - num) in hashmap:
            return [i, hasmap.get(target-num)]
        else:
            hashmap[num] = i
```

## Tips

### 哈希表

>哈希表（[Hash Table](https://leetcode-cn.com/tag/hash-table/)，也叫散列表），是根据键（Key）而直接访问在内存存储位置的数据结构。也就是说，它通过计算一个关于键值的函数，将所需查询的数据映射到表中一个位置来访问记录，这加快了查找速度。这个映射函数称做哈希函数，存放记录的数组称做哈希表。
>
>哈希表是使用 $$O(1)$$时间进行数据的插入删除和查找，但是哈希表不保证表中数据的有序性，这样在哈希表中查找最大数据或者最小数据的时间是 $$O(N)$$ 实现。

python的数据结构`dict()`是hashmap的一种实现。

### dict.get(key)方法和dict[key]获取键对应的值

在获取不到相应值时，前者会返回None；后者报错。

### enumerate()

> [enumerate()](https://www.runoob.com/python/python-func-enumerate.html) 函数用于将一个可遍历的数据对象(如列表、元组或字符串)组合为一个索引序列，同时列出数据和数据下标，一般用在 for 循环当中。
>
> 使用方法：
>
> ```python
> enumerate(sequence, [start=0])
> # 举例
> >>>seasons = ['Spring', 'Summer', 'Fall', 'Winter']
> >>> list(enumerate(seasons))
> [(0, 'Spring'), (1, 'Summer'), (2, 'Fall'), (3, 'Winter')]
> >>> list(enumerate(seasons, start=1))       # 下标从 1 开始
> [(1, 'Spring'), (2, 'Summer'), (3, 'Fall'), (4, 'Winter')]
> ```
>
> 

