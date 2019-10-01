---
title: '每天一道Leetcode: 1. Two Sum'
copyright: true
tags: leetcode
categories: leetcode
abbrlink: d6d87edd
date: 2019-09-22 11:10:27
---
Two Sum
<!--more-->
Given an array of integers, return indices of the two numbers such that they add up to a specific target.

You may assume that each input would have exactly one solution, and you may not use the same element twice.

**Example:**

Given nums = [2, 7, 11, 15], target = 9,

Because nums[0] + nums[1] = 2 + 7 = 9,
return [0, 1].



* thought process:
    

1. 暴力法 O(N^2) O(1)

Java Code:
```
class Solution {
    public int[] twoSum(int[] nums, int target) {
             for(int i=0;i<nums.length;i++)
             {
                for(int j=i+1;j<nums.length;j++)
                {
                      if(nums[j]==target-nums[i])
                        return new int[]{i,j};
                }
             }
             
throw new IllegalArgumentException("No two sum solution");
    }
}
```

****Optimization method****
2. Hashmap  O(N) O(N)

Java Code
```
class Solution {
    public int[] twoSum(int[] nums, int target) {
        Map<Integer,Integer> map=new HashMap<>();
        for(int i=0;i<nums.length;i++){
             map.put(nums[i], i);         
        }
            
        for(int i=0;i<nums.length;i++)
        {
            int ans=target-nums[i];
            if(map.containsKey(ans)&&map.get(ans)!=i)
            {
                return new int[]{i, map.get(ans)};
            }
        }
        throw new IllegalArgumentException("No two sum solution");
    }
}
```
3. 最优解 hashmap
Java Code
```
class Solution {
    public int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> map = new HashMap<>();
        for (int i = 0; i < nums.length; i++) {
            int complement = target - nums[i];
            if (map.containsKey(complement)) {
                return new int[] { map.get(complement), i };
            }
            map.put(nums[i], i);
        }
        throw new IllegalArgumentException("No two sum solution");
    }
}
```