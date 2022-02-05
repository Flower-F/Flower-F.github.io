---
title: N 皇后问题
date: 2022-01-29 11:24:23
tags: [leetcode, 算法]
copyright: true
---
题目链接：
https://leetcode-cn.com/problems/n-queens/

解法分析：
```js
/**
 * @param {number} n
 * @return {string[][]}
 */
var solveNQueens = function (n) {
  const board = new Array(n).fill('.').map(() => new Array(n).fill('.'));
  const res = [];
  backtrack(0);
  return res;

  function backtrack(row) {
    if (row === n) {
      const copyBoard = board.slice();
      for (let i = 0; i < n; i++) 
      copyBoard[i] = copyBoard[i].join('');
      res.push(copyBoard);
      return;
    }

    for (let col = 0; col < n; col++) {
      if (!isValid(row, col)) {
        continue;
      }
      board[row][col] = 'Q';
      backtrack(row + 1);
      board[row][col] = '.';
    }
  }

  function isValid(row, col) {
    // 因为是从上往下一行一行放的，所以不需要下方的行
    for (let i = 0; i < row; i++) {
      for (let j = 0; j < n; j++) {
        // 如果发现了 Q，并且和自己同列或同对角线
        // 判断同对角线，使用 |y1 - y2| === |x1 - x2|
        if (board[i][j] === 'Q') {
          if (j === col || Math.abs(i - row) === Math.abs(j - col)) {
            return false;
          }
        }
      }
    }
    return true;
  }
};
```