---
title: 每天一道LeetCode：9. Palindrome Number
copyright: true
tags: leetcode
categories: leetcode
abbrlink: e52671c4
date: 2019-10-01 00:43:39
---
Palindrome Number
<!--more-->
Determine whether an integer is a palindrome. An integer is a palindrome when it reads the same backward as forward.

**Example 1:**

Input: 121
Output: true

**Example 2:**

Input: -121
Output: false
Explanation: From left to right, it reads -121. From right to left, it becomes 121-. Therefore it is not a palindrome.

**Example 3:**

Input: 10
Output: false
Explanation: Reads 01 from right to left. Therefore it is not a palindrome.

Follow up:

Coud you solve it without converting the integer to a string?
****Thought Process****



**Java Code**
强制转换后，使用内置函数 reverse
```
class Solution {
    public boolean isPalindrome(int x) {
        String reversedStr = (new StringBuilder(x + "")).reverse().toString();
        return (x + "").equals(reversedStr);
    }
}
```
但是该种方法涉及到Java里面的类型强转，比较耗性能的，每一次强转都是需要拆箱装箱的。


**Solution Two：**
**优化算法**
通过取整和取余操作获取整数中对应的数字进行比较。

举个例子：1551 这个数字。

 *   通过计算 1551 / 1000， 得首位1
 *   通过计算 1551 % 10， 可得末位 1
 *   进行比较
 *   再将 55 取出来继续比较

Code：
```
class Solution {
    public boolean isPalindrome(int x) {
        if(x<0) return false;
        int div=1;
        
        while(x/div>=10) div*=10;
        while(x>0){
            int left=x/div;
            int right=x%10;
            if(left!=right) return false;
            x=(x%div)/10;
            div/=100;
        }
        return true;
    }
}
```

**Solution Three**
**取出后半段数字进行反转，判断反转后的数字与前半段数字是否相等**

* 每次取余，y=x%10;(前半段)
* 每次将y(取出的最小数)加到后半段的数字上 revertNumber=revertNumber\*10+y;
* 每取一个y，同时也需要x/10
* 然后判断x 是否小于revertNumber,如果小于，则数字已经对半分了。
* 最后，判断奇偶数，如果为偶数,revertNum==x; 如果是奇数，最中间的数字就在revertNum的最低位上，将它除以10以后应当与x相等。

**Coding:**
```
class Solution {
    public boolean isPalindrome(int x) {
        if(x<0||(x%10==0&&x!=0)) return false;
      
        int revertNumber=0;
        while(x>revertNumber){
            revertNumber=revertNumber*10+x%10;
            x/=10;
        }
        return  x==revertNumber||x==revertNumber/10;
    }
}
```
