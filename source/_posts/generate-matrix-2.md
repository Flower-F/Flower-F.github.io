---
title: 螺旋矩阵 2
date: 2021-12-13 11:54:17
tags: [算法, leetcode]
copyright: true
---
题目链接：
https://leetcode-cn.com/problems/spiral-matrix-ii/
解法分析：纯模拟，注意细节
```ts
function generateMatrix(n: number): number[][] {
    const maxNum = n * n;
    let curNum = 1;
    const direction = [[0, 1], [1, 0], [0, -1], [-1, 0]]; // 顺时针
    let col = 0, row = 0;
    let directionIndex = 0;
    const res = new Array(n).fill(0).map(() => new Array(n).fill(0));

    while (curNum <= maxNum) {
        res[row][col] = curNum;
        curNum++;

        const nextRow = row + direction[directionIndex][0];
        const nextCol = col + direction[directionIndex][1];

        if (!(nextRow >= 0 && nextRow < n && nextCol >= 0 && nextCol < n &&
            res[nextRow][nextCol] === 0)) {
            directionIndex = (directionIndex + 1) % 4; // 顺时针旋转
        }

        row = row + direction[directionIndex][0];
        col = col + direction[directionIndex][1];
    }

    return res;
};
```