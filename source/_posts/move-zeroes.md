---
title: 移动零
date: 2021-12-09 17:45:53
tags: [算法, codetop]
copyright: true
---
题目链接：
https://leetcode-cn.com/problems/move-zeroes/
解法分析：双指针问题。本题使用的是快速排序的思想，快速排序是设定一个基准，然后小于等于的数放在基准左边，大于的放在基准右边。在这道题目里面，0 就是基准数，不等于 0 的就放在左边，等于 0 的就放在右边。

```ts
/**
 Do not return anything, modify nums in-place instead.
 */
 function moveZeroes(nums: number[]): void {
    let left = 0, right = 0;
    while(right < nums.length) {
        if(nums[right] !== 0) {
            // 右指针的数不等于 0 ，需要进行交换
            [nums[left], nums[right]] = [nums[right], nums[left]];
            // 交换完成之后，左指针的数变为非零数字，因此左指针需要右移
            left++;
        }
        right++;
    }
};
```