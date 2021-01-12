---
layout: post
title: median-of-two-sorted-arrays
date: 2021-01-11 14:55:19
tags:
	- N3:LeetCode
categories: [Algorithm]
---

## 题目

[4. 寻找两个正序数组的中位数](https://leetcode-cn.com/problems/median-of-two-sorted-arrays/)

>给定两个大小为 m 和 n 的正序（从小到大）数组 nums1 和 nums2。请你找出并返回这两个正序数组的中位数。
>
>**进阶**：你能设计一个时间复杂度为 O(log (m+n)) 的算法解决此问题吗？
>
> 示例 1：
>
>```
>输入：nums1 = [1,3], nums2 = [2]
>输出：2.00000
>解释：合并数组 = [1,2,3] ，中位数 2
>```
>
>示例 2：
>
>```
>输入：nums1 = [1,2], nums2 = [3,4]
>输出：2.50000
>解释：合并数组 = [1,2,3,4] ，中位数 (2 + 3) / 2 = 2.5
>```
>
>示例 3：
>
>```
>输入：nums1 = [0,0], nums2 = [0,0]
>输出：0.00000
>```
>
>示例 4：
>
>```
>输入：nums1 = [], nums2 = [1]
>输出：1.00000
>```
>
>示例 5：
>
>```
>输入：nums1 = [2], nums2 = []
>输出：2.00000
>```

<!-- More -->

## 思路

~~光荣的~~第一道困难题啊……

因为示例给足了暗(wu)示(dao)，所以直觉上肯定是合并数组然后取中位数。时间复杂度$O(m+n)$。

在题目的**进阶**提示中，要求时间复杂度达到$O(log(m+n))$，看到$log(n)$级别的复杂度，应当想到**二分法**。

## Python实现

首先是最原始的做法：

```python
def findMedianSortedArrays(self, nums1: List[int], nums2: List[int]) -> float:
    mergednums = []
    pos1 = 0
    pos2 = 0
    m = len(nums1)
    n = len(nums2)
    # 合并数组
    while pos1 < m or pos2 < n:
        if pos1 < m and pos2 < n:
            if nums1[pos1] < nums2[pos2]:
                mergednums.append(nums1[pos1])
                pos1 += 1
            elif nums1[pos1] > nums2[pos2]:
                mergednums.append(nums2[pos2])
                pos2 += 1
            else:
                mergednums.append(nums1[pos1])
                mergednums.append(nums2[pos2])
                pos1 += 1
                pos2 += 1
        elif pos1 == m:
            mergednums[len(mergednums):(len(mergednums)+n)] = nums2[pos2:(n)]
            pos2 = n
        elif pos2 == n:
            mergednums[len(mergednums):(len(mergednums)+m)] = nums1[pos1:(m)]
            pos1 = m

    # 返回中位数
    if len(mergednums)%2 == 1:
        return mergednums[len(mergednums)//2]
    elif len(mergednums) == 0:
        return 0
    else:
        return (1/2)*(mergednums[(len(mergednums)-1)//2] + mergednums[len(mergednums)//2])
```

二分法：TODO

## Tips

### 二分法

TODO