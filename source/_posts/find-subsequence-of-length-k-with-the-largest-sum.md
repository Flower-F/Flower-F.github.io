---
title: 找到和最大的长度为 K 的子序列
date: 2021-12-12 00:32:10
tags: [算法, 周赛]
copyright: true
---
第一次参加周赛，比赛时间内只写出了第一题，比赛结束半小时后过了第三题。算法能力还是太菜了，不过第一次参加周赛的体验感觉还是不错的。因为没有题解，所以能更专心地投入到题目中。没有标签，也需要自己去判断需要使用什么算法。想起了大一那段不堪回首的 ACM 集训队生活，最终以失败结束，但是过程还是让我学到了一些。发骚结束，进入正题。

题目链接：
https://leetcode-cn.com/problems/find-subsequence-of-length-k-with-the-largest-sum/
解法分析：先按值排序，后按下标排序
```js
/**
 * @param {number[]} nums
 * @param {number} k
 * @return {number[]}
 */
var maxSubsequence = function (nums, k) {
    const numsWithIndex = [];
    for (let i = 0; i < nums.length; i++) {
        // 存储下标
        numsWithIndex[i] = {
            index: i,
            val: nums[i]
        }
    }
    // 按值排序
    numsWithIndex.sort((a, b) => (b.val - a.val));

    // 筛选出前 k 项
    const res = [];
    for (let i = 0; i < k; i++) {
        res.push(numsWithIndex[i]);
    }

    // 按下标排序
    res.sort((a, b) => (a.index - b.index));

    // 筛选出对应的值
    const ans = res.map(item => item.val);
    return ans;
};
```