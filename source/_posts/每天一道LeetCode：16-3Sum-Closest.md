---
title: 每天一道LeetCode：16. 3Sum Closest
copyright: true
tags: leetcode
categories: leetcode
abbrlink: 179757ff
date: 2019-10-12 13:07:43
---
3Sum Closest
<!--more-->
Given an array nums of n integers and an integer target, find three integers in nums such that the sum is closest to target. Return the sum of the three integers. You may assume that each input would have exactly one solution.

**Example:**

Given array nums = [-1, 2, 1, -4], and target = 1.

The sum that is closest to the target is 2. (-1 + 2 + 1 = 2).

排序+双指针

Java Code
```

class Solution {
public int threeSumClosest(int[] nums, int target) {
Arrays.sort(nums);
int ans=nums[0]+nums[1]+nums[2];
for(int i=0;i<nums.length;i++){
int start=i+1,end=nums.length-1;
while(start<end){
int sum=nums[start]+nums[end]+nums[i];
if(Math.abs(target-ans)>Math.abs(target-sum))
ans=sum;
if(sum>target) end--;
else if(sum<target) start++;
else return ans;
}
}
return ans;
}
}


```