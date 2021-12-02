---
title: 合并两个有序数组
date: 2021-12-02 14:53:23
tags: [算法, codetop]
copyright: true
---
题目链接：
https://leetcode-cn.com/problems/merge-sorted-array/
解法分析：双指针，但是考虑到可能会覆盖的问题，所以要从后面倒着遍历

```js
/**
 * @param {number[]} nums1
 * @param {number} m
 * @param {number[]} nums2
 * @param {number} n
 * @return {void} Do not return anything, modify nums1 in-place instead.
 */
var merge = function(nums1, m, nums2, n) {
    // 双指针 数组末尾
    let i = m - 1, j = n - 1, tail = nums1.length - 1;
    // 记录当前要存放的值
    let cur;
    while(i >= 0 || j >= 0) {
        if(i < 0) {
            cur = nums2[j--];
        } else if(j < 0) {
            cur = nums1[i--];
        } else if(nums1[i] > nums2[j]) {
            cur = nums1[i--];
        } else {
            cur = nums2[j--];
        }
        nums1[tail--] = cur;
    }
    return nums1;
};
```