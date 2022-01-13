---
title: 长度最小的子数组
date: 2022-01-13 12:26:58
tags: [算法, leetcode]
copyright: true
---
题目链接：
https://leetcode-cn.com/problems/minimum-size-subarray-sum/
解法分析：滑动窗口
```js
/**
 * @param {number} target
 * @param {number[]} nums
 * @return {number}
 */
var minSubArrayLen = function(target, nums) {
    let result = Number.MAX_SAFE_INTEGER;
    let sum = 0; // 滑动窗口数值和
    let left = 0; // 滑动窗口起始位置

    for (let right = 0; right < nums.length; right++) {
        sum += nums[right];
        // 检验答案是否需要更改
        while (sum >= target) {
            result = Math.min(result, right - left + 1); // 更新答案
            sum -= nums[left++]; //  不断缩小窗口大小（滑动窗口的核心）
        }
    }
    return result === Number.MAX_SAFE_INTEGER ? 0 : result;
};
```