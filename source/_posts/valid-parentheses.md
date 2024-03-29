---
title: "有效的括号"
date: 2019-12-08 16:12:50
tags: 
    - leetcode
categories: [Algorithm]
---

在TODO LIST里面躺了很久的刷题，终于~~槽点颇多地~~来了……  
最近很信奉的话是，要清楚自己什么行、什么不行。所以就从简单题开始吧。

做题顺序：[azl397985856 - leetcode 经典题目的解析](https://github.com/azl397985856/leetcode#leetcode-%E7%BB%8F%E5%85%B8%E9%A2%98%E7%9B%AE%E7%9A%84%E8%A7%A3%E6%9E%90)  
解析指路：[azl397985856 - 20.validParentheses.md](https://github.com/azl397985856/leetcode/blob/master/problems/20.validParentheses.md)

## 题目

[20. 有效的括号](https://leetcode-cn.com/problems/valid-parentheses/)

>给定一个只包括 `'('`，`')'`，`'{'`，`'}'`，`'['`，`']'` 的字符串，判断字符串是否有效。  
>  
>有效字符串需满足：  
>  
>左括号必须用相同类型的右括号闭合。  
>左括号必须以正确的顺序闭合。  
>注意空字符串可被认为是有效字符串。  
>  
>**示例 1:**  
```
输入: "()"  
输出: true  
```
>**示例 2:**  
```
输入: "()[]{}"  
输出: true  
```
>**示例 3:**  
```
输入: "(]"  
输出: false  
```
>**示例 4:**
```
输入: "([)]"  
输出: false  
```
>**示例 5:**  
```
输入: "{[]}"  
输出: true  
```

<!-- More -->

## 分析

很经典的题目了，说到左右括号匹配首先是应该想到**栈**的。具体一点说，是遍历字符串，遇左括号则入栈，遇右括号则与栈顶元素匹配。  
匹配时，先从简单的判断出发，考虑如何判断括号不匹配：不匹配一定发生在右括号要与栈顶匹配时，栈空或不匹配。此时直接退出循环，将结果设为`false`。  
P.S.这里我的一个问题是没有**退出循环**，甚至还将左括号出栈，继续循环。反例：`[[)]`  
那么接下来在考虑返回true的情况，一定是匹配成功**前提下**栈空。  
P.S.做题中间主要考虑不周是在这里，首先如果直接输入`""`(就是什么都不输入)，应该是返回true的，也就是结果的初始值应该设置为true(当时拍脑袋设置了false)；其次，一定不是最后匹配成功就能返回true，还需要判断栈空与否，很简单的反例是`(()`。  
这里和上面匹配失败不及时退出是比较大的两个失误。

所以，在遍历字符串每个字符时，分以下情况讨论：  

1. 无括号：返回`true`；

2. 左括号：直接入栈，此时结果设为`false`；  

3. 右括号：首先判断栈是否为空：

- 如果栈为空，直接返回`false`，并且`break`；

- 如果栈非空，栈顶出栈，判断是否与栈顶匹配：

    - 如不匹配，直接返回`false`，并且`break`；

    - 如果匹配，判断栈是否为空，如为空则设为`true`，如不为空则设为`false`；

## 代码

`match()`和`verifyFlag()`主要参考《数据结构——Java语言描述（清华大学出版社）》教材，并不是必须要抽出来这两个方法。  

### JAVA实现  

<small>首先挂一个窒息的失误，大概一年没写JAVA的我，居然拿字符串和字符判相等于否。  
退学吧: )</small>

```java
class Solution {
    public final int LEFT = 0;
    public final int RIGHT = 1;

    public boolean match(String in_stack, String out_stack) {
        if ((in_stack.equals("(") && out_stack.equals(")")) ||
                (in_stack.equals("[") && out_stack.equals("]")) ||
                (in_stack.equals("{") && out_stack.equals("}")) ) {
            return true;
        } else {
            return false;
        }
    }

    public int verifyFlag(String s) {
        int FLAG = -1;
        if (s.equals("(")  ||
                s.equals("[") ||
                s.equals("{") ) {
                    FLAG = LEFT;
        } else if(s.equals(")")  ||
                s.equals("]") ||
                s.equals("}") ) {
                    FLAG = RIGHT;
        }
        return FLAG;
    }

    public boolean isValid(String s) {
        int s_length = s.length();
        Stack stack = new Stack();
        boolean result = true;  // 1. 无括号： true
        for (int i = 0; i < s_length; i++) {
            String each_s = String.valueOf(s.charAt(i));
            if (verifyFlag(each_s) == LEFT) {  // 2. 左括号：入栈 false 不必break
                stack.push(each_s);
                result = false;
            } else if (verifyFlag(each_s) == RIGHT) {// 3. 右括号
                if (stack.isEmpty()) {  //3.1 栈空：false 必须break；
                    result = false;
                    break;
                } else {  //3.2 栈非空
                    String stack_top = String.valueOf(stack.pop());
                    if (!match(stack_top, each_s)) {  // 3.2.1 不匹配：false 必须break；
                        result = false;
                        break;
                    } else {  // 3.2.2 匹配 true
                        if (stack.isEmpty()) {
                            result = true;
                        }
                    }
                }
            }
        }
        return result;
    }
}
```

### C++实现  

<small>暂时打算把JAVA强行翻译成++，用这种方法来过++的语法。指针等部分再补吧。</small>

```C++
#include <stack>
using namespace std;

class Solution {
    public:
        int LEFT = 0;
        int RIGHT = 1;

        bool match(string in_stack, string out_stack) {
            if ((in_stack.compare("(") == 0 && out_stack.compare(")") == 0) ||
                    (in_stack.compare("[") == 0 && out_stack.compare("]") == 0) ||
                    (in_stack.compare("{") == 0 && out_stack.compare("}") == 0) ) {
                return true;
            } else {
                return false;
            }
        }

        int verifyFlag(string s) {
            int Flag = -1;
            if (s.compare("(") == 0  ||
                    s.compare("[") == 0 ||
                    s.compare("{") == 0 ) {
                        Flag = LEFT;
            } else if(s.compare(")") == 0  ||
                    s.compare("]") == 0 ||
                    s.compare("}") == 0 ) {
                        Flag = RIGHT;
            }
            return Flag;
        }

        bool isValid(string s) {
            int s_length = s.length();
            stack<string> stk;
            bool result = true;  // 1. 无括号： true
            for (int i = 0; i < s_length; i++) {
                string each_s = s.substr(i,1);
                if (verifyFlag(each_s) == LEFT) {  // 2. 左括号：入栈 false 不必break
                    stk.push(each_s);
                    result = false;
                } else if (verifyFlag(each_s) == RIGHT) {// 3. 右括号
                    if (stk.empty()) {  //3.1 栈空：false 必须break；
                        result = false;
                        break;
                    } else {  //3.2 栈非空
                        string stk_top = stk.top();
                        stk.pop();
                        if (match(stk_top, each_s)) {  // 3.2.1 匹配 true
                            if (stk.empty()) {
                                result = true;
                            }
                        } else {  // 3.2.2 不匹配：false 必须break；
                            result = false;
                            break;
                        }
                    }
                }
            }
            return result;
        }
};
```

#### Tips for `++`  

1. `bool`是布尔值的声明关键字；
2. C++判断字符串是否相等的方法：`str.compare("") == 0`，相当于JAVA的`compareTo()`；
3. 关于string类型，首先++的string类的声明是用的`string`，是小写；  
其次切割字符数组的方法是`str.substr(pos,len)`，`pos`指位置下标，`len`指分割出的长度；  
4. 关于stack类型，判断栈空的方法为`empty()`，而不是JAVA的`isEmpty()`；  
其次栈顶元素通过`stk.top()`获得，这个方法不会让栈顶元素出栈；同时，`stk.pop()`方法让栈顶元素出栈但是并不会返回栈顶元素。  

#### TODO

++指针