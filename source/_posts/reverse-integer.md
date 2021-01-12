---
layout: post
title: "整数反转"
date: 2021-01-12 14:27:25
tags:
	- N3:LeetCode
categories: [Algorithm]
---

## 题目

[7. 整数反转](https://leetcode-cn.com/problems/reverse-integer/)

>给出一个 32 位的有符号整数，你需要将这个整数中每位上的数字进行反转。
>
>**注意**：
>
>假设我们的环境只能存储得下 32 位的有符号整数，则其数值范围为 $$[−2^{31},  2^{31} − 1]$$。请根据这个假设，如果反转后整数溢出那么就返回 0。
>
>
>示例 1：
>
>```
>输入：x = 123
>输出：321
>```
>
>示例 2：
>
>```
>输入：x = -123
>输出：-321
>```
>
>示例 3：
>
>```
>输入：x = 120
>输出：21
>```
>
>示例 4：
>
>```
>输入：x = 0
>输出：0
>```

<!-- More -->

## 分析

通过求10的余数来依次取个位数（不要想多，用不着栈的）。

这道题目的难点并不在于取数，而在于判断是否溢出。解决这个问题的方法之一是在循环的过程中判断每次的结果除以十是否与上次相同，我在Java实现部分使用了这种方法。至于Python……Python本身就是弱类型语言，直接判断数字是不是溢出就行……

## Java实现

```java
public int reverse(int x) {
    int x1 = Math.abs(x);
    int res = 0;
    while (x1 != 0) {
        int t = res;
        res = x1 % 10 + 10 * res;
        x1 = x1 / 10;
        if(res / 10 != t){  // 判断溢出
            return 0;
        }
    }
    return (x >= 0) ? res : (-1) * res;
}
```



## Python实现

```python
def reverse(self, x: int) -> int:
    res = 0
    x1 = abs(x)
    while x1 != 0:
        pop = x1 % 10
        x1 = x1 // 10
        res = 10*res + pop
    res = res if x >= 0 else (-1)*res
    return res if -2 ** 31 < res < 2 ** 31 - 1 else 0
```

## Tips

### 三目运算符

三目运算符其实不应该在这里才来总结，但是既然遇到了，还是写一写。

Java中，三目运算符是`（判断语句）?（语句为真的返回结果）:（语句为假的返回结果）`；在Python中是`（语句为真的返回结果）if（判断语句）else（语句为假的返回结果）`。

> 你才学了多少，就能把不同语言的语法混在一起了……