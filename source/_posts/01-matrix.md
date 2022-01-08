---
title: 01矩阵
date: 2021-12-17 11:34:44
tags: [算法, leetcode]
copyright: true
---
题目链接：
https://leetcode-cn.com/problems/01-matrix/
解法分析：从 0 的位置开始进行广度优先搜索。广度优先搜索可以找到从起点到其余所有点的最短距离，因此如果我们从 0 开始搜索，每次搜索到一个 1，就可以得到 0 到这个 1 的最短距离，也就是离这个 1 最近的 0 的距离了。

```ts
interface Point {
    x: number;
    y: number;
}

function updateMatrix(mat: number[][]): number[][] {
    const directions = [[0, 1], [0, -1], [1, 0], [-1, 0]];
    const row = mat.length, col = mat[0].length;
    const visited = new Array<boolean>(row).fill(false).map(() => new Array<boolean>(col).fill(false));
    const dist = new Array<number>(row).fill(0).map(() => new Array<number>(col).fill(0));
    const queue: Point[] = [];

    // 将所有的 0 添加进初始队列中
    for (let i = 0; i < row; i++) {
        for (let j = 0; j < col; j++) {
            if (mat[i][j] === 0) {
                queue.push({
                    x: i,
                    y: j
                });
                visited[i][j] = true;
            }
        }
    }

    // bfs
    while (queue.length) {
        const { x, y } = queue.shift() as Point;
        for (let i = 0; i < 4; i++) {
            const newX = x + directions[i][0];
            const newY = y + directions[i][1];

            // 边界判断
            if (newX >= 0 && newX < row && newY >= 0 && newY < col && !visited[newX][newY]) {
                dist[newX][newY] = dist[x][y] + 1;
                queue.push({
                    x: newX,
                    y: newY
                });
                visited[newX][newY] = true;
            }
        }
    }

    return dist;
};
```

参考题解：
https://leetcode-cn.com/problems/01-matrix/solution/01ju-zhen-by-leetcode-solution/