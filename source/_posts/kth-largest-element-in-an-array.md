---
title: 数组中的第 K 个最大元素
date: 2022-01-08 18:59:39
tags: [算法, codetop]
copyright: true
---
题目链接：
https://leetcode-cn.com/problems/kth-largest-element-in-an-array/

解法分析：快排变种
```js
/**
 * @param {number[]} nums
 * @param {number} k
 * @return {number}
 */
var findKthLargest = function (nums, k) {
    let left = 0, right = nums.length - 1;
    let target = nums.length - k;
    while (left <= right) {
        const index = partition(nums, left, right);
        if (index === target) {
            return nums[index];
        } else if (index < target) {
            left = index + 1;
        } else if (index > target) {
            right = index - 1;
        }
    }
    return -1;

    function partition(nums, left, right) {
        if (right > left) {
            let randomIndex = Math.floor(Math.random() * (right - left)) + left + 1;
            [nums[left], nums[randomIndex]] = [nums[randomIndex], nums[left]];
        }

        const pivot = nums[left];
        let i = left, j = right;

        while (i < j) {
            while (nums[j] >= pivot && i < j) j--;
            while (nums[i] <= pivot && i < j) i++;

            [nums[i], nums[j]] = [nums[j], nums[i]];
        }
        [nums[i], nums[left]] = [nums[left], nums[i]];

        return i;
    }
};
```

参考题解：
https://leetcode-cn.com/problems/kth-largest-element-in-an-array/solution/partitionfen-er-zhi-zhi-you-xian-dui-lie-java-dai-/