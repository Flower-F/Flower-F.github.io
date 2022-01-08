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
等过几天我考完笔试就补上