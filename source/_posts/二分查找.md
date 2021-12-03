---
title: 二分查找
date: 2021-12-03 17:48:24
tags: [算法, codetop]
copyright: true
---
题目链接：
https://leetcode-cn.com/problems/binary-search
解法分析：标准二分查找
```js
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number}
 */
var search = function(nums, target) {
    let left = 0, right = nums.length - 1;
    while(left <= right) {
        const mid = (left + right) >> 1;
        if(nums[mid] === target) {
            return mid;
        } else if(nums[mid] > target) {
            right = mid - 1;
        } else {
            left = mid + 1;
        }
    }
    return -1;
};
```