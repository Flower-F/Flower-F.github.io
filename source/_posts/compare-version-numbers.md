---
title: 比较版本号
date: 2022-01-08 16:01:50
tags: [codetop, 算法]
copyright: true
---
题目链接：
https://leetcode-cn.com/problems/compare-version-numbers/
解法分析：
1. 分割字符串

```js
/**
 * @param {string} version1
 * @param {string} version2
 * @return {number}
 */
var compareVersion = function (version1, version2) {
    const nums1 = version1.split('.'), nums2 = version2.split('.');
    const len1 = nums1.length, len2 = nums2.length;
    for (let i = 0; i < len1 || i < len2; i++) {
        let num1, num2;
        num1 = i < len1 ? Number(nums1[i]) : 0;
        num2 = i < len2 ? Number(nums2[i]) : 0;

        if (num1 > num2) {
            return 1;
        } else if (num1 < num2) {
            return -1;
        }
    }
    return 0;
};
```

2. 双指针
用于优化空间复杂度

```js
/**
 * @param {string} version1
 * @param {string} version2
 * @return {number}
 */
var compareVersion = function(version1, version2) {
    const len1 = version1.length, len2 = version2.length;
    let i = 0, j = 0;
    while (i < len1 || j < len2) {
        let x = 0;
        for (; i < len1 && version1[i] !== '.'; i++) {
            x = x * 10 + parseInt(version1[i]);
        }
        i++; // 跳过 .
        let y = 0;
        for (; j < len2 && version2[j] !== '.'; j++) {
            y = y * 10 + parseInt(version2[j]);
        }
        j++; // 跳过 .
        if (x > y) {
            return 1;
        } else if (x < y) {
            return -1;
        }
    }
    return 0;
};
```