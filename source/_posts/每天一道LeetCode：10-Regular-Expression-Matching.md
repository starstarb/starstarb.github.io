---
title: 每天一道LeetCode：10. Regular Expression Matching
copyright: true
tags: leetcode
categories: leetcode
abbrlink: 8262ebee
date: 2019-10-01 08:45:31
---
Regular Expression Matching
<!--more-->
Given an input string (s) and a pattern (p), implement regular expression matching with support for '.' and '\*'.

'.' Matches any single character.
'* ' Matches zero or more of the preceding element.

The matching should cover the entire input string (not partial).

**Note:**

    s could be empty and contains only lowercase letters a-z.
    p could be empty and contains only lowercase letters a-z, and characters like . or *.

**Example 1**:

Input:
s = "aa"
p = "a"
Output: false
Explanation: "a" does not match the entire string "aa".

Example 2:

Input:
s = "aa"
p = "a*"
Output: true
Explanation: '* '   means zero or more of the preceding element, 'a'. Therefore, by repeating 'a' once, it becomes "aa".


**Example 3:**

Input:
s = "ab"
p = ".\*"
Output: true
Explanation: ".\*" means "zero or more (\*) of any character (.)".

**Example 4:**

Input:
s = "aab"
p = "c*a*b"
Output: true
Explanation: c can be repeated 0 times, a can be repeated 1 time. Therefore, it matches "aab".

**Example 5:**

Input:
s = "mississippi"
p = "mis\*is\*p*."
Output: false

****Thought Process****



**Java Code**

直接回溯法：
```
public class Solution {
    public boolean isMatch(String text, String pattern) {
        if (pattern.isEmpty()) return text.isEmpty();
        //第一个是否匹配上
        boolean first_match = (!text.isEmpty() &&
            (pattern.charAt(0) == text.charAt(0) || pattern.charAt(0) == '.'));
        if (pattern.length() >= 2 && pattern.charAt(1) == '*') {
            //看有没有可能,剩下的pattern匹配上全部的text
            //看有没有可能,剩下的text匹配整个pattern
            return (isMatch(text, pattern.substring(2)) ||
                (first_match && isMatch(text.substring(1), pattern)));
        } else {
            //没有星星的情况:第一个字符相等,而且剩下的text,匹配上剩下的pattern
            return first_match && isMatch(text.substring(1), pattern.substring(1));
        }
    }
}
```
动态规划（DP），找到方程式和边界条件即可