---
title: 每天一道LeetCode：3.Longest Substring Without Repeating Characters
copyright: true
tags: leetcode
categories: leetcode
abbrlink: 3f88f749
date: 2019-09-24 17:23:15
---
Longest Substring Without Repeating Characters
<!--more-->
Given a string, find the length of the longest substring without repeating characters.

**Example 1:**

Input: "abcabcbb"
Output: 3 
Explanation: The answer is "abc", with the length of 3. 

**Example 2:**

Input: "bbbbb"
Output: 1
Explanation: The answer is "b", with the length of 1.

**Example 3:**

Input: "pwwkew"
Output: 3
Explanation: The answer is "wke", with the length of 3. 


Note that the answer must be a substring, "pwke" is a subsequence and not a substring.


****Thought Process****



**Java Code**
1. 暴力法
```
class Solution {
    public int lengthOfLongestSubstring(String s) {
        int  n=s.length();
        int ans=0;
        for(int i=0;i<n;i++)
            for(int j=i+1;j<=n;j++)
                if(allUnique(s,i,j)) ans=Math.max(ans,j-i);
        return ans;
    }
    public boolean allUnique(String s,int start,int end){
        Set<Character> set=new HashSet<>();
        for(int i=start;i<end;i++)
        {
            Character ch=s.charAt(i);
            if(set.contains(ch)) return false;
            set.add(ch);
         }
    return true;  
     }
}
```
2. 滑动窗口
对于暴力解法，会出现遍历字符串很多次，因此，可以优化算法。

如何优化？ 减少遍历次数，如果从索引i到 j-1之间的子字符串已经被检查出没
有重复字符，那么只需要检查s[j]对应的字符是否已经存在于子字符串中。

新算法：滑动窗口，窗口通常是在数组或者字符串中由开始到结束索引定义的一系列元素的集合，即[i,j)（左闭右开）， 滑动即例如：我们将[i,j)向左滑动一个元素，则它将变为[i+1,j+1)（左闭右开）

Java中使用HashSet将字符存储在当前窗口[i,j)，然后向右滑动索引j，如果不在HashSet中，那么继续滑动j 直到s[j]存在于HashSet中。


此时，我们找到的没有重复字符的最长子字符串将会以索引 i 开头。如果我们对所有的 i 这样做，就可以得到答案。

```
class Solution {
    public int lengthOfLongestSubstring(String s) {
        int n=s.length();
        Set<Character> set=new HashSet<>();
        int ans=0,i=0,j=0;
        while(i<n&&j<n){
            if(!set.contains(s.charAt(j))){
                set.add(s.charAt(j++));
                ans=Math.max(ans,j-i);
            }
            else{
              set.remove(s.charAt(i++));
            }
        }
        return ans;
    }
}

```

3. 优化的滑动窗口
   我们可以定义字符到索引的映射，而不是使用集合来判断一个字符是否存在。 当我们找到重复的字符时，我们可以立即跳过该窗口。

也就是说，如果 s[j]  在 [i,j)  范围内有与 j′ 重复的字符，我们不需要逐渐增加 i 。 我们可以直接跳过 [i，j′] 范围内的所有元素，并将 i变为 j′+1。

使用HashMap
```
class Solution {
    public int lengthOfLongestSubstring(String s) {
        int n=s.length();
        int ans=0;
        Map<Character,Integer> map =new HashMap<>();
        for(int i=0,j=0;j<n;j++){
            if(map.containsKey(s.charAt(j)))
                i=Math.max(map.get(s.charAt(j)),i);
            ans=Math.max(ans,j-i+1);
            map.put(s.charAt(j),j+1);
        }
         return ans;
    }
   
}

```
