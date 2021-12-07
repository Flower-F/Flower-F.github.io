---
title: 两个数组的交集
date: 2021-12-07 22:12:59
tags: [算法, codetop]
copyright: true
---
题目链接：
https://leetcode-cn.com/problems/intersection-of-two-arrays/
解法分析：使用 set 存储其中一个数组的所有数字，然后查询另一个数组中的数字是否存在 set 中即可，结果的保存也需要使用 set，以防出现重复记录的情况。最后用 Array.from 或者展开运算符将 set 转换为数组返回即可。

```js
/**
 * @param {number[]} nums1
 * @param {number[]} nums2
 * @return {number[]}
 */
var intersection = function(nums1, nums2) {
    if(nums1.length === 0 || nums2.length === 0) {
        return [];
    }
    const set = new Set();
    for(let i = 0; i < nums1.length; i++) {
        if(!set.has(nums1[i])) {
            set.add(nums1[i]);
        }
    }
    
    const res = new Set();
    for(let i = 0; i < nums2.length; i++) {
        if(set.has(nums2[i])) {
            res.add(nums2[i]);
        }
    }
    
    return Array.from(res);
    // return [...res];
};
```