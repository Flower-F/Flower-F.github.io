---
title: 二分查找 II
date: 2022-01-08 12:39:27
tags: [算法, nowcoder]
copyright: true
---
题目链接：
https://www.nowcoder.com/practice/4f470d1d3b734f8aaf2afb014185b395
解法分析：二分变形，主要是 while 判断条件里面变为必须严格小于

```js
/**
 * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
 *
 * 如果目标值存在返回下标，否则返回 -1
 * @param nums int整型一维数组 
 * @param target int整型 
 * @return int整型
 */
function search(nums, target) {
    if (nums.length === 0) {
        return -1;
    }
    // write code here
    let left = 0, right = nums.length - 1;
    while (left < right) {
        const mid = (left + right) >> 1;
        if (nums[mid] > target) {
            right = mid - 1;
        } else if (nums[mid] < target) {
            left = left + 1;
        } else {
            // mid 右侧一定不是答案，但 mid 有可能是
            right = mid;
        }
    }
    
    if (nums[left] === target) return left;
    
    return -1;
}
module.exports = {
    search: search
};
```