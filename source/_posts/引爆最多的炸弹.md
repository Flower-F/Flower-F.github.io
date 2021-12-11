---
title: 引爆最多的炸弹
date: 2021-12-12 00:46:37
tags: [算法, 周赛]
copyright: true
---
题目链接：
https://leetcode-cn.com/problems/detonate-the-maximum-bombs/
解法分析：先构建图，再用 bfs 的模板
```js
/**
 * @param {number[][]} bombs
 * @return {number}
 */
var maximumDetonation = function (bombs) {
    const len = bombs.length;
    
    // 构建图
    const graph = getGraph(bombs);

    let ans = 0;

    // bfs
    for (let i = 0; i < len; i++) {
        const visitd = new Array(len).fill(false);
        const queue = [];
        queue.push(i);
        visitd[i] = true;
        let count = 0;
        while (queue.length > 0) {
            count++;
            const cur = queue.shift();
            for (let j of graph[cur]) {
                if (visitd[j]) continue;
                visitd[j] = true;
                queue.push(j);
            }
        }
        ans = Math.max(ans, count);
    }

    return ans;

    function getGraph() {
        const res = [];
        for (let i = 0; i < len; i++) {
            const [x, y, r] = bombs[i];
            res[i] = [];
            for (let j = 0; j < len; j++) {
                if (i === j) {
                    continue;
                }
                const [x1, y1, r1] = bombs[j];
                const dist = (x - x1) * (x - x1) + (y - y1) * (y - y1);
                // 点在圆内
                if (dist <= r * r) {
                    res[i].push(j);
                }
            }
        }

        return res;
    }
};
```