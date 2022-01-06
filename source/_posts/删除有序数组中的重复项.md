---
title: 删除有序数组中的重复项
date: 2022-01-06 10:44:37
tags: [算法, leetcode]
copyright: true
---
题目链接：
https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array/
解法分析：读写指针。
- 开始时读写指针一起指向数字第一个元素
- 当读写指针指向的数字相同时，读指针前移
- 当读写指针不一致时，写指针前移，并写入数据

```js
/**
 * @param {number[]} nums
 * @return {number}
 */
var removeDuplicates = function (nums) {
    let rp, wp; // 读指针，写指针
    rp = wp = 0;

    while (rp < nums.length) {
        if (nums[rp] !== nums[wp]) {
            wp++;
            nums[wp] = nums[rp];
        } else {
            rp++;
        }
    }

    return wp + 1;
};
```