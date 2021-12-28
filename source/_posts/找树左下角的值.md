---
title: 找树左下角的值
date: 2021-12-28 15:09:44
tags: [算法, leetcode]
copyright: true
---
题目链接：
https://leetcode-cn.com/problems/find-bottom-left-tree-value/
解法分析：DFS，与 `求根节点到叶节点数字之和` 一题很相似。

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
 * @return {number}
 */
var findBottomLeftValue = function(root) {
    let res, maxDepth = 0;
    dfs(root, 0);
    return res;

    function dfs(root, depth) {
        if (!root) {
            return;
        }
        const curDepth = depth + 1;
        if (curDepth > maxDepth) {
            maxDepth = curDepth;
            res = root.val;
        }
        dfs(root.left, curDepth);
        dfs(root.right, curDepth);
    }
};
```