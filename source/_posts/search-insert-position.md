---
title: 搜索插入位置
date: 2022-01-07 11:46:14
tags: [算法, leetcode]
copyright: true
---
题目链接：
https://leetcode-cn.com/problems/search-insert-position/
解法分析：简单二分

```js
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number}
 */
var searchInsert = function (nums, target) {
    let left = 0, right = nums.length - 1;
    while (left <= right) {
        const mid = (left + right) >> 1;
        if (nums[mid] > target) {
            right = mid - 1;
        } else if (nums[mid] < target) {
            left = left + 1;
        } else {
            return mid;
        }
    }
    return left;
};
```