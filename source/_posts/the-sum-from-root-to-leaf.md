---
title: 求根节点到叶节点数字之和
date: 2021-12-26 10:27:23
tags: [算法, leetcode]
copyright: true
---
题目链接：
https://leetcode-cn.com/problems/sum-root-to-leaf-numbers/
解法分析：DFS

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
var sumNumbers = function(root) {
    if (!root) {
        return 0;
    }
    let res = 0;
    dfs(root, 0);
    return res;

    function dfs(root, sum) {
        if (!root) {
            return;
        }

        sum = sum * 10 + root.val;

        // 到达叶子节点
        if (!root.left && !root.right) {
            res += sum;
        }

        dfs(root.left, sum);
        dfs(root.right, sum);
    }
};
```