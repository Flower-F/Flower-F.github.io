---
title: 岛屿数量
date: 2021-12-16 19:38:20
tags: [算法, leetcode]
copyright: true
---
题目链接:
https://leetcode-cn.com/problems/number-of-islands/
解法分析：dfs 经典题目，注意边界判断。

```ts
function numIslands(grid: string[][]): number {
    if (grid === null || grid.length === 0) {
        return 0;
    }
    const directions = [[1, 0], [0, 1], [-1, 0], [0, -1]];
    const row = grid.length, col = grid[0].length;
    let count = 0;
    // 遍历所有点
    for (let i = 0; i < row; i++) {
        for (let j = 0; j < col; j++) {
            if (grid[i][j] === '1') {
                dfs(i, j);
                count++;
            }
        }
    }
    return count;

    function dfs(x: number, y: number) {
        // 判断边界
        if (x < 0 || y < 0 || x >= row || y >= col || grid[x][y] === '0') {
            return;
        }
        // 将点设置为 0，标记已经遍历过
        grid[x][y] = '0';
        for (let i = 0; i < 4; i++) {
            dfs(x + directions[i][0], y + directions[i][1]);
        }
    }
};
```