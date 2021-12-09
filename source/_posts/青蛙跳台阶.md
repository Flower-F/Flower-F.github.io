---
title: 青蛙跳台阶
date: 2021-12-05 22:02:08
tags: [算法, codetop]
copyright: true
---
题目链接：
https://leetcode-cn.com/problems/qing-wa-tiao-tai-jie-wen-ti-lcof/
解法分析：纯的斐波那契

```js
/**
 * @param {number} n
 * @return {number}
 */
var numWays = function(n) {
    if(n === 1 || n === 0) {
        return 1;
    }
    if(n === 2) {
        return 2;
    }
    
    let prev1 = 1, prev2 = 2, cur;
    for(let i = 3; i <= n; i++) {
        cur = (prev1 + prev2) % 1000000007;
        prev1 = prev2;
        prev2 = cur;
    }
    
    return cur;
};
```