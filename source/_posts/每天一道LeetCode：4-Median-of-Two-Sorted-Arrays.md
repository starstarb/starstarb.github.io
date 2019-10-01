---
title: 每天一道LeetCode：4. Median of Two Sorted Arrays
copyright: true
tags: leetcode
categories: leetcode
abbrlink: '61434630'
date: 2019-09-25 23:06:14
---
Median of Two Sorted Arrays
<!--more-->
There are two sorted arrays nums1 and nums2 of size m and n respectively.

Find the median of the two sorted arrays. The overall run time complexity should be O(log (m+n)).

You may assume nums1 and nums2 cannot be both empty.

**Example 1:**

nums1 = [1, 3]
nums2 = [2]

The median is 2.0

**Example 2:**

nums1 = [1, 2]
nums2 = [3, 4]

The median is (2 + 3)/2 = 2.5

****Thought Process****
 合并两个数组，两个有序数组的合并也是归并排序中的一部分。

如果为数组1为空，然后看数组2是否是偶数，求中位数。
同理，数组2同上。

如果均不为空，那么定义一个while循环，合并数组，直到
 计数标志  count=m+n（数组1+数组2）
 跳出循环后，判断合并后的数组偶数/奇数，找出中位数。
 但是时间复杂度为O(m+n)，题目过了，但是不满足时间复杂度条件。
 
 
**Java Code**
**原暴力算法**
```
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
    int[] nums;
    int m = nums1.length;
    int n = nums2.length;
    nums = new int[m + n];
    if (m == 0) {
        if (n % 2 == 0) {
            return (nums2[n / 2 - 1] + nums2[n / 2]) / 2.0;
        } else {

            return nums2[n / 2];
        }
    }
    if (n == 0) {
        if (m % 2 == 0) {
            return (nums1[m / 2 - 1] + nums1[m / 2]) / 2.0;
        } else {
            return nums1[m / 2];
        }
    }

    int count = 0;
    int i = 0, j = 0;
    while (count != (m + n)) {
        if (i == m) {
            while (j != n) {
                nums[count++] = nums2[j++];
            }
            break;
        }
        if (j == n) {
            while (i != m) {
                nums[count++] = nums1[i++];
            }
            break;
        }

        if (nums1[i] < nums2[j]) {
            nums[count++] = nums1[i++];
        } else {
            nums[count++] = nums2[j++];
        }
    }

    if (count % 2 == 0) {
        return (nums[count / 2 - 1] + nums[count / 2]) / 2.0;
    } else {
        return nums[count / 2];
    }
}


}

```

**第一次优化代码：**

其实，我们不需要将两个数组真的合并，我们只需要找到中位数在哪里就可以了。

开始的思路是写一个循环，然后里边判断是否到了中位数的位置，到了就返回结果，但这里对偶数和奇数的分类会很麻烦。当其中一个数组遍历完后，出了 for 循环对边界的判断也会分几种情况。总体来说，虽然复杂度不影响，但代码会看起来很乱。

首先是怎么将奇数和偶数的情况合并一下。

用 len 表示合并后数组的长度，如果是奇数，我们需要知道第 （len+1）/2 个数就可以了，如果遍历的话需要遍历 int(len/2 ) + 1 次。如果是偶数，我们需要知道第 len/2和 len/2+1 个数，也是需要遍历 len/2+1 次。所以遍历的话，奇数和偶数都是 len/2+1 次。

返回中位数的话，奇数需要最后一次遍历的结果就可以了，偶数需要最后一次和上一次遍历的结果。所以我们用两个变量 left 和 right，right 保存当前循环的结果，在每次循环前将 right 的值赋给 left。这样在最后一次循环的时候，left 将得到 right 的值，也就是上一次循环的结果，接下来 right 更新为最后一次的结果。

循环中该怎么写，什么时候 A 数组后移，什么时候 B 数组后移。用 aStart 和 bStart 分别表示当前指向 A 数组和 B 数组的位置。如果 aStart 还没有到最后并且此时 A 位置的数字小于 B 位置的数组，那么就可以后移了。也就是aStart＜m&&A[aStart]< B[bStart]。

但如果 B 数组此刻已经没有数字了，继续取数字 B[ bStart ]，则会越界，所以判断下 bStart 是否大于数组长度了，这样 || 后边的就不会执行了，也就不会导致错误了，所以增加为 aStart＜m&&(bStart) >= n||A[aStart]<B[bStart]) 。

```
class Solution {
   public double findMedianSortedArrays(int[] A, int[] B) {
    int m = A.length;
    int n = B.length;
    int len = m + n;
    int left = -1, right = -1;
    int aStart = 0, bStart = 0;
    for (int i = 0; i <= len / 2; i++) {
        left = right;
        if (aStart < m && (bStart >= n || A[aStart] < B[bStart])) {
            right = A[aStart++];
        } else {
            right = B[bStart++];
        }
    }
    if ((len & 1) == 0)
        return (left + right) / 2.0;
    else
        return right;
}

}
```
时间复杂度：遍历 len/2+1 次，len=m+n，所以时间复杂度依旧是 O(m+n))。

空间复杂度：我们申请了常数个变量，也就是 m，n，len，left，right，aStart，bStart 以及 i。

总共 8 个变量，所以空间复杂度是 O(1）


**第二次优化代码**
   给出官方代码

遇见log(m+n)的时间复杂度，一般都是二分解决
```
class Solution {
    public double findMedianSortedArrays(int[] A, int[] B) {
        int m = A.length;
        int n = B.length;
        if (m > n) { // to ensure m<=n
            int[] temp = A; A = B; B = temp;
            int tmp = m; m = n; n = tmp;
        }
        int iMin = 0, iMax = m, halfLen = (m + n + 1) / 2;
        while (iMin <= iMax) {
            int i = (iMin + iMax) / 2;
            int j = halfLen - i;
            if (i < iMax && B[j-1] > A[i]){
                iMin = i + 1; // i is too small
            }
            else if (i > iMin && A[i-1] > B[j]) {
                iMax = i - 1; // i is too big
            }
            else { // i is perfect
                int maxLeft = 0;
                if (i == 0) { maxLeft = B[j-1]; }
                else if (j == 0) { maxLeft = A[i-1]; }
                else { maxLeft = Math.max(A[i-1], B[j-1]); }
                if ( (m + n) % 2 == 1 ) { return maxLeft; }

                int minRight = 0;
                if (i == m) { minRight = B[j]; }
                else if (j == n) { minRight = A[i]; }
                else { minRight = Math.min(B[j], A[i]); }

                return (maxLeft + minRight) / 2.0;
            }
        }
        return 0.0;
    }
}
```