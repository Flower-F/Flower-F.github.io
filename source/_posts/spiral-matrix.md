---
title: 螺旋矩阵 I && 螺旋矩阵 II
date: 2021-12-13 11:54:17
tags: [算法, leetcode]
copyright: true
---
# 螺旋矩阵 I
题目链接：
https://leetcode-cn.com/problems/spiral-matrix/
```js
/**
 * @param {number[][]} matrix
 * @return {number[]}
 */
var spiralOrder = function(matrix) {
    const m = matrix.length, n = matrix[0].length;
    const visited = new Array(m).fill(false).map(() => new Array(n).fill(false));
    const directions = [[0, 1], [1, 0], [0, -1], [-1, 0]];
    let row = 0, col = 0;
    let directionIndex = 0;
    const res = [];

    for (let i = 1; i <= m * n; i++) {
        res.push(matrix[row][col]);
        visited[row][col] = true;
        const newRow = row + directions[directionIndex][0], 
            newCol = col + directions[directionIndex][1];
        if (newRow >= m || newRow < 0 || newCol >= n || newCol < 0 || visited[newRow][newCol]) {
            directionIndex = (directionIndex + 1) % 4;
        }
        row = row + directions[directionIndex][0];
        col = col + directions[directionIndex][1];
    }

    return res;
};
```

# 螺旋矩阵 II
题目链接：
https://leetcode-cn.com/problems/spiral-matrix-ii/
解法分析：几乎同螺旋矩阵 I
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