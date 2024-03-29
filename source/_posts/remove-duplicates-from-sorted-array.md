---
title: "删除排序数组中的重复项"
date: 2019-12-09 19:52:02
tags: 
    - leetcode
categories: [Algorithm]
---

做题顺序：[azl397985856 - leetcode 经典题目的解析](https://github.com/azl397985856/leetcode#leetcode-%E7%BB%8F%E5%85%B8%E9%A2%98%E7%9B%AE%E7%9A%84%E8%A7%A3%E6%9E%90)  

题目：[26. 删除排序数组中的重复项](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array/)  
解析指路：[azl397985856 - 26.remove-duplicates-from-sorted-array.md](https://github.com/azl397985856/leetcode/blob/master/problems/26.remove-duplicates-from-sorted-array.md)

## 题目

>给定一个排序数组，你需要在**原地**删除重复出现的元素，使得每个元素只出现一次，返回移除后数组的新长度。
>
>不要使用额外的数组空间，你必须在**原地修改输入数组**并在使用 O(1) 额外空间的条件下完成。
>
>**示例 1**:
```
给定数组 nums = [1,1,2], 

函数应该返回新的长度 2, 并且原数组 nums 的前两个元素被修改为 1, 2。 

你不需要考虑数组中超出新长度后面的元素。
```
>**示例 2**:
```
给定 nums = [0,0,1,1,1,2,2,3,3,4],

函数应该返回新的长度 5, 并且原数组 nums 的前五个元素被修改为 0, 1, 2, 3, 4。

你不需要考虑数组中超出新长度后面的元素。
```
>**说明**:
>
>为什么返回数值是整数，但输出的答案是数组呢?
>
>请注意，输入数组是以“**引用**”方式传递的，这意味着在函数里修改输入数组对于调用者是可见的。
>
>你可以想象内部操作如下:
```
// nums 是以“引用”方式传递的。也就是说，不对实参做任何拷贝
int len = removeDuplicates(nums);

// 在函数里修改输入数组对于调用者是可见的。
// 根据你的函数返回的长度, 它会打印出数组中该长度范围内的所有元素。
for (int i = 0; i < len; i++) {
    print(nums[i]);
}
```

<!-- More -->

## 分析

开始做的时候觉得蛮简单的一题，反复读了两遍题目构思了一下，发现真的蛮简单的。  
我的思路是：首先设置一个新数组长度`count_len`的初始值，然后遍历数组，将相邻的两数相比较；如果相等，则不必改变新数组长度值`count_len`，并继续遍历；如果不等，则说明该数要移到`count_len`后面，同时`count_len`需要`+1`。  
做完查了一查，发现这种算法叫做**快慢指针**。概括一下，我的方法中的`i`就是快指针，而`count_len`充当的是慢指针的角色。（因为是排序数组，快慢指针之间只有相等的数，所以不管是我的方法，即比较数组的i和i-1位置，还是快慢指针的比较，都是相同的思路）  
快慢指针的直观之处在于这两个指针直接标志了原数组和新数组的最后项。

## 代码

### JAVA实现  

<small>今天是用纸笔写代码还一遍提交通过的一天~</small>

```java
class Solution {
    public int removeDuplicates(int[] nums) {
        int count_len = 1;
        for(int i = 1; i < nums.length; i++) {
            if(nums[i] == nums[i-1]) {  // 重复项出现，不需要操作新数组的长度
                continue;
                // System.out.println(String.valueOf(nums[i]) + " is the same with the previous.");
            } else {
                count_len++;
                if (count_len - 1 == i) {  // 该数已经在正确的位置，不需要交换位置
                    // System.out.println(String.valueOf(nums[i]) + " is bigger but need not change position.");
                    continue;
                } else {  // 将数字移到重复项前
                    nums[count_len - 1] = nums[i];
                    // System.out.println(String.valueOf(nums[i]) + " is bigger and will change position to " + String.valueOf(count_len - 1));
                }
            }
        }
        if (nums.length < count_len) {  // 考虑空数组情况
            count_len = nums.length;
        }
    return count_len;
    }
}
```

### C++实现  

```C++
class Solution {  // 对JAVA中的代码进行了适当的简化，将所有continue而没有执行具体判断的if-else语句都砍掉了
public:
    int removeDuplicates(vector<int>& nums) {
        int count_len = 1;
        for(int i = 1; i < nums.size(); i++) {
            if ( (nums[i] != nums[i-1]) && (count_len - 1 != i) ) {
                count_len++;
                nums[count_len - 1] = nums[i];
            }
        }
        if (nums.size() < count_len) {
            count_len = nums.size();
        }
    return count_len;
    }
};
```

#### Tips for `++`  

1. `vector.size()`返回`vector`长度。