---
title: 回旋镖的数量
date: 2022-01-01 11:35:49
tags: [算法, leetcode]
copyright: true
---
题目链接：
https://leetcode-cn.com/problems/number-of-boomerangs/
解法分析：
- 当我们固定其中一个点 A 的时候，并且想算距离为 3 的点的个数，那么我们就找出所有和点 A 距离为 3 的点，然后使用排列组合计算 A(N, 2) 即可（N 个点中选 2 个点）
- 因为需要统计元素频率自然想到使用哈希表
细节：由于题目要求了需要考虑元组的顺序，因此枚举的时候两层的下标都需要从 0 开始

```js
/**
 * @param {number[][]} points
 * @return {number}
 */
var numberOfBoomerangs = function (points) {
    const len = points.length;
    if (len < 3) {
        return 0;
    }

    let ans = 0;
    for (let i = 0; i < len; i++) {
        const map = new Map();
        for (let j = 0; j < len; j++) {
            const dis = dis2(points[i], points[j]);
            map.set(dis, (map.get(dis) || 0) + 1);
        }
        map.forEach(value => {
            // 排列组合
            // A(n, 2)
            ans += value * (value - 1);
        })
    }

    return ans;

    function dis2(p1, p2) {
        return (p1[0] - p2[0]) * (p1[0] - p2[0]) + (p1[1] - p2[1]) * (p1[1] - p2[1]);
    }
};
```