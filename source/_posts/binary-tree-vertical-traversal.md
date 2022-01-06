---
title: 二叉树的垂序遍历
date: 2021-12-29 21:39:35
tags: [算法, leetcode]
copyright: true
---
题目链接：
https://leetcode-cn.com/problems/vertical-order-traversal-of-a-binary-tree/
解法分析：先 DFS 遍历，再按照题意排序，最后根据列整理出答案。

```js
/**
 * Definition for a binary tree node.
 * function TreeNode(val, left, right) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.left = (left===undefined ? null : left)
 *     this.right = (right===undefined ? null : right)
 * }
 */
/**
 * @param {TreeNode} root
 * @return {number[][]}
 */
var verticalTraversal = function (root) {
    const nodes = [];
    dfs(root, 0, 0, nodes);

    nodes.sort((a, b) => {
        if (a.col !== b.col) {
            // 列从左到右
            return a.col - b.col;
        } else if (a.row !== b.row) {
            // 行从上到下
            return a.row - b.row;
        } else {
            // 同行同列，按值从小到大
            return a.val - b.val;
        }
    });

    // 按照列来整理答案
    const res = [];
    let lastCol = Number.MIN_SAFE_INTEGER;
    for (const node of nodes) {
        const { col, val } = node;
        if (col !== lastCol) {
            lastCol = col;
            res.push([]);
        }
        res[res.length - 1].push(val);
    }

    return res;

    function dfs(root, row, col) {
        if (!root) {
            return;
        }
        nodes.push({
            col,
            row,
            val: root.val
        })
        dfs(root.left, row + 1 , col - 1);
        dfs(root.right, row + 1, col + 1);
    }
};
```