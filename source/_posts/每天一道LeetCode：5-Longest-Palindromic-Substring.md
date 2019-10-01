---
title: 每天一道LeetCode：5. Longest Palindromic Substring
copyright: true
tags: leetcode
categories: leetcode
abbrlink: 253bbf24
date: 2019-09-26 22:14:51
---
 Longest Palindromic Substring
<!--more-->
Given a string s, find the longest palindromic substring in s. You may assume that the maximum length of s is 1000.

**Example 1:**

Input: "babad"
Output: "bab"
Note: "aba" is also a valid answer.

**Example 2:**

Input: "cbbd"
Output: "bb"

****Thought Process****



**Java Code**
1. 纯暴力算法，直接两重for循环判断i 和n-i之间的子字符串是否是回文字符串，用Math.max函数取最长回文子串
```
class Solution {
      public boolean lp(String s){
     int  n=s.length();
        for(int i=0;i<n/2;i++){
            if(s.charAt(i)!=s.charAt(n-i-1)){
                return  false;
            }
        }
        return true;
     }
    public String longestPalindrome(String s) {
        
        String ans="";
        int max=0;
        int n=s.length();
        for(int i=0;i<n;i++){
            for(int j=i+1;j<=n;j++){
                String t=s.substring(i,j);
                if( lp(t) && t.length()>max){
                    ans=s.substring(i,j);
                    max=Math.max(max,ans.length());
                }
            }
            
        }
        return ans;
    }
}
```
时间复杂度： O(n^3)
空间复杂度： O(1)
2. 优化算法
动态规划
```
class Solution {
   public String longestPalindrome(String s) {
    int n = s.length();
    String res = "";
    boolean[][] dp = new boolean[n][n];
    for (int i = n - 1; i >= 0; i--) {
        for (int j = i; j < n; j++) {
            dp[i][j] = s.charAt(i) == s.charAt(j) && (j - i < 2 || dp[i + 1][j - 1]);         
            if (dp[i][j] &&  j - i + 1 > res.length()) {
                res = s.substring(i, j + 1);
            }
        }
    }
    return res;
}

}
```
还有更多优化算法，以后再来复习