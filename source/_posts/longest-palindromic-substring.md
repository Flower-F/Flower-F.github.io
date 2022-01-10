---
title: 最长回文子串
date: 2022-01-09 20:57:23
tags: [leetcode, 算法]
copyright: true
---
题目链接：
https://leetcode-cn.com/problems/longest-palindromic-substring/
解法分析：暴力模拟，还可以用马拉车，但是太复杂，我记不住（

```js
/**
 * @param {string} s
 * @return {string}
 */
var longestPalindrome = function(s) {
    let res = '';
    for (let i = 0; i < s.length; i++) {
        // 长度为奇数：以 s[i] 为中心的最长回文子串
        const s1 = palindrome(s, i, i);
        // 长度为偶数：以 s[i] 和 s[i + 1] 为中心的最长回文子串
        const s2 = palindrome(s, i , i + 1);
        res = res.length > s1.length ? res : s1;
        res = res.length > s2.length ? res : s2;
    }
    return res;

    // 设置左右两个指针，是为了可以同时处理回文串长度为奇数和偶数的情况
    function palindrome(s, l, r) {
        while (l >= 0 && r < s.length && s[l] === s[r]) {
            l--;
            r++;
        }

        // 返回以 s[l] 和 s[r] 为中心的最长回文串
        return s.substring(l + 1, r);
    }
};
```