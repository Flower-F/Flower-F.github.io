---
title: 找到小镇的法官
date: 2022-01-09 09:59:02
tags: [leetcode, 算法]
copyright: true
---
题目链接：
https://leetcode-cn.com/problems/find-the-town-judge/
解法分析：
我们可以将小镇中的人们之间的信任关系抽象为图的边，这样问题就转化为求图中入度为 n - 1 并且出度为 0 的点。进一步简化，根据前面的分析，显然可知若存在法官，则 n - 1 为出度与入度差的最大值，因此可以直接求解**是否存在出度和入度差为 n - 1 的图**即可。

```js
/**
 * @param {number} n
 * @param {number[][]} trust
 * @return {number}
 */
var findJudge = function(n, trust) {
    if (trust.length === 0 && n === 1) {
        return 1;
    }
    const counter = new Map();
    for (const path of trust) {
        counter.set(path[0], (counter.get(path[0]) || 0) - 1); // 入度
        counter.set(path[1], (counter.get(path[1]) || 0) + 1); // 出度
    }
    for (const elem of counter) {
        if (elem[1] === n - 1) {
            return elem[0];
        }
    }
    return -1;
};
```