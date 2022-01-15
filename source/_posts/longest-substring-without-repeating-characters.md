---
title: 无重复字符的最长子串
date: 2022-01-08 15:01:37
tags: [leetcode, 算法]
copyright: true
---
题目链接：
https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/
解法分析：滑动窗口
```js
/**
 * @param {string} s
 * @return {number}
 */
var lengthOfLongestSubstring = function(s) {
    const map = new Map();
    let maxLen = 0;
    let left = 0;
    for (let i = 0; i < s.length; i++) {
        // 若出现重复字符
        if (map.has(s[i])) {
            // 比如对于字符串 abca，当前有效最长子段是 abc，接着我们又遍历到第二个 a
            // 那么此时更新 left 为 map.get(a) + 1 = 1，当前有效子段就更新为了 bca
            left = Math.max(left, map.get(s[i]) + 1);
        }

        // 此处覆盖前面的 key 也没关系，因为我们要求的是最长子串
        // 如果在小范围内包括该 key，则大范围内也必定包含该 key
        // 即若小范围不成立，大范围必定也不成立
        map.set(s[i], i);
        maxLen = Math.max(maxLen, i - left + 1);
    }

    return maxLen;
};
```

参考题解：
https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/solution/hua-dong-chuang-kou-by-powcai/