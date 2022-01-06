---
title: 将数组分成和相等的三部分
date: 2021-12-11 17:57:38
tags: [算法, codetop]
copyright: true
---
题目链接：
https://leetcode-cn.com/problems/partition-array-into-three-parts-with-equal-sum/
解法分析：前面部分的思路都很简单，主要说一下最后一行代码为什么要写 >= 而不是 ===，这是因为些特别坑的情况，比如 [0, 0, 0, 0]，或者是像 [-1, 1, -1, 1, -1, 1, -1, 1] 这样的情况。可以看出，当和为 0 的时候，可以分成大于三等份的情况必定也可以分成刚好三等份。
```js
/**
 * @param {number[]} arr
 * @return {boolean}
 */
var canThreePartsEqualSum = function(arr) {
    const sum = arr.reduce((pre, cur) => pre + cur);
    
    if(sum % 3 !== 0) {
        return false;
    }
    
    const part = sum / 3;
    let count = 0;
    let ans = 0;
    for(let i = 0; i < arr.length; i++) {
        count += arr[i];
        if(count === part) {
            ans++;
            count = 0;
        }
    }
    
    return ans >= 3;
};
```