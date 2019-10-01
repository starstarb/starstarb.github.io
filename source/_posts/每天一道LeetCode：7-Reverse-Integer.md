---
title: 每天一道LeetCode：7. Reverse Integer
copyright: true
abbrlink: 48d54608
date: 2019-09-28 12:14:02
tags: leetcode
categories: leetcode
---
Reverse Integer
<!--more-->
Given a 32-bit signed integer, reverse digits of an integer.

**Example 1:**

Input: 123
Output: 321

**Example 2:**

Input: -123
Output: -321

**Example 3:**

Input: 120
Output: 21

Note:
Assume we are dealing with an environment which could only store integers within the 32-bit signed integer range: [−2^31,  2&31 − 1]. For the purpose of this problem, assume that your function returns 0 when the reversed integer overflows.


****Thought Process****
反转整数， 利用堆栈，代码如下：
```
pop =x%10;
x/=10;


push = rev*10+pop;
rev=push;

```
但是 `push=rev*10+pop`容易溢出，
因此需要检查溢出范围。

   溢出条件有两个，一个是大于整数最大值MAX_VALUE，另一个是小于整数最小值MIN_VALUE，设当前计算结果为rev，下一位为pop。
   
   
   1. 从rev * 10 + pop > MAX_VALUE这个溢出条件来看

   当出现 ans > MAX_VALUE / 10 且 还有pop需要添加 时，则一定溢出
   
   当出现 ans == MAX_VALUE / 10 且 pop > 7 时，则一定溢出，7是2^31 - 1的个位数

   2. 从ans * 10 + pop < MIN_VALUE这个溢出条件来看
   
   当出现 ans < MIN_VALUE / 10 且 还有pop需要添加 时，则一定溢出
    
   当出现 ans == MIN_VALUE / 10 且 pop < -8 时，则一定溢出，8是-2^31的个位数


**Java Code**
```
class Solution {
    public int reverse(int x) {
        int rev=0;
        while(x!=0){
            int pop=x%10;
            x/=10;
            if(rev>Integer.MAX_VALUE/10||(rev==Integer.MAX_VALUE/10&&pop>7))
                return 0;
            if(rev<Integer.MIN_VALUE/10||(rev==Integer.MIN_VALUE/10&&pop<-8))
                return 0;
            rev=rev*10+pop;
        }
        return rev;
    }
}

```