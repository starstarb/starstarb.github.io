---
title: 每天一道LeetCode：6. ZigZag Conversion
copyright: true
tags: leetcode
categories: leetcode
abbrlink: 4a37bc2c
date: 2019-09-28 12:11:42
---
ZigZag Conversion
<!--more-->
The string "PAYPALISHIRING" is written in a zigzag pattern on a given number of rows like this: (you may want to display this pattern in a fixed font for better legibility)

P   A   H   N
A P L S I I G
Y   I   R

And then read line by line: "PAHNAPLSIIGYIR"

Write the code that will take a string and make this conversion given a number of rows:

string convert(string s, int numRows);

Example 1:

Input: s = "PAYPALISHIRING", numRows = 3
Output: "PAHNAPLSIIGYIR"

Example 2:

Input: s = "PAYPALISHIRING", numRows = 4
Output: "PINALSIGYAHRPI"
Explanation:

P     I    N
A   L S  I G
Y A   H R
P     I

****Thought Process****

**字符串数组**
整体的思路是遍历字符串，遍历过程中将每行都看成新的字符串构成字符串数组，最后再将该数组拼接起来即可

如果 numRows=1则说明当前字符串即为结果，直接返回

否则整个字符串需要经历，向下向右，向下向右，这样的反复循环过程，设定 down变量表示是否向下，loc 变量表示当前字符串数组的下标

如果 down 为 true，则 loc+=1，字符串数组下标向后移动，将当前字符加入当前字符串中

如果 down为 false，则表示向右，则 loc−=1，字符串数组下标向前移动，将当前字符加入当前字符串中

时间复杂度：O(n)，n为字符串s的长度

**Java Code**

```
class Solution {
    public String convert(String s, int numRows) {
        if(numRows==1)
            return s;
        int len=Math.min(s.length(),numRows);
        String []rows=new String[len];
        for(int i=0;i<len;i++)
            rows[i]="";
        int loc=0;
        boolean down=false;
        for(int i=0;i<s.length();i++){
            rows[loc]+=s.substring(i,i+1);
             if(  loc==0 || loc==numRows-1)
                 down=!down;
            loc+=down?1:-1;
        }
        String ans="";
        for(String row:rows){
            ans+=row;
        }
        return ans;
    }
}
```