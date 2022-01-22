---
title: N 数之和
date: 2022-01-22 17:05:25
tags: [leetcode, 算法]
copyright: true
---
# [两数之和](https://leetcode-cn.com/problems/two-sum/)

```js
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number[]}
 */
var twoSum = function(nums, target) {
  const map = new Map();

  for (let i = 0; i < nums.length; i++) {
    if (map.has(target - nums[i])) {
      return [i, map.get(target - nums[i])];
    } else {
      map.set(nums[i], i);
    }
  }
};
```

# [三数之和](https://leetcode-cn.com/problems/3sum/)

```js
/**
 * @param {number[]} nums
 * @return {number[][]}
 */
var threeSum = function(nums) {
  if (nums.length < 3) {
    return [];
  }
  const res = [];
  nums = nums.sort((a, b) => a - b);

  // 循环遍历第一个数，转变为两数之和
  for (let i = 0; i < nums.length; i++) {
    // 剪枝，如果这一项大于 0， 后面的必然也大于 0
    if (nums[i] > 0) {
      return res;
    }
    // 当前遍历的数字与上一个遍历的数字相同，为避免重复所以跳过
    if (i > 0 && nums[i] === nums[i - 1]) {
      continue;
    }
    // 双指针
    let left = i + 1, right = nums.length - 1;
    while (left < right) {
      if (nums[left] + nums[right] + nums[i] > 0) {
        // 两个指针之和太大，右指针左移
        right--;
      } else if (nums[left] + nums[right] + nums[i] < 0) {
        // 两个指针之和太小，左指针右移
        left++;
      } else {
        // 找到了和为零的组合
        res.push([nums[i], nums[left], nums[right]]);
        // 左右指针向内缩小
        left++;
        right--;
        // 去重
        // 例如：[-4,1,1,1,2,3,3,3], i=0, left=1, right=5
        while (left < right && nums[left - 1] === nums[left]) left++;
        while (left < right && nums[right + 1] === nums[right]) right--;
      }
    }
  }

  return res;
};
```

