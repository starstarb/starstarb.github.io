---
title: 每天一道LeetCode：15. 3Sum
copyright: true
tags: leetcode
categories: leetcode
abbrlink: fd359b29
date: 2019-10-07 12:10:58
---
3Sum
<!--more-->
Given an array nums of n integers, are there elements a, b, c in nums such that a + b + c = 0? Find all unique triplets in the array which gives the sum of zero.

**Note:**

The solution set must not contain duplicate triplets.

**Example:**

Given array nums = [-1, 0, 1, 2, -1, -4],

A solution set is:
[
  [-1, 0, 1],
  [-1, -1, 2]
]

* 首先对数组进行排序，排序后固定一个数 nums[i]，再使用左右指针指向 nums[i]后面的两端，数字分别为 nums[L]和 nums[R]，计算三个数的和 sum 判断是否满足为 0，满足则添加进结果集
* 如果 nums[i]大于 0，则三数之和必然无法等于 0，结束循环
* 如果 nums[i]== nums[i−1]，则说明该数字重复，会导致结果重复，所以应该跳过
* 当 sum == 0 时，nums[L] == nums[L+1] 则会导致结果重复，应该跳过，L++
* 当 sum == 0时，nums[R] == nums[R−1]则会导致结果重复，应该跳过，R−−
时间复杂度：O(n^2)，n 为数组长度

Java Code:
```
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        List<List<Integer>> ans=new ArrayList();
        int len=nums.length;
        if(nums==null||len<3) return ans;
        Arrays.sort(nums);
        for(int i=0;i<len;i++){
            if(nums[i]>0)break;
            if(i>0&&nums[i]==nums[i-1]) continue;
            int L=i+1;
            int R=len-1;
            while(L<R){
                int sum=nums[i]+nums[L]+nums[R];
                if(sum==0){
                ans.add(Arrays.asList(nums[i],nums[L],nums[R]));
                    while(L<R&&nums[L]==nums[L+1])L++;
                    while(L<R&&nums[R]==nums[R-1])R--;
                    L++;
                    R--;
                }
                else if(sum<0)L++;
                else if(sum>0)R--;
            }
        }
        return ans;
    }
}
```