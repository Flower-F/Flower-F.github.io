---
title: 相同的树
date: 2021-12-25 13:47:50
tags: [算法, leetcode]
copyright: true
---
题目链接：
https://leetcode-cn.com/problems/same-tree/
解法分析：递归，两棵树相同当它们的左子树相同且右子树相同。

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
 * @param {TreeNode} p
 * @param {TreeNode} q
 * @return {boolean}
 */
var isSameTree = function(p, q) {
    if (!p && !q) {
        return true;
    }
    if (!p || !q) {
        return false;
    }

    return p.val === q.val && isSameTree(p.left, q.left) && isSameTree(p.right, q.right);
};
```