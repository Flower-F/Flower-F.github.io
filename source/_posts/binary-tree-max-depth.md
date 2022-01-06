---
title: 二叉树的最大深度
date: 2021-12-03 18:31:03
tags: [算法, codetop]
copyright: true
---
题目链接：
https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/
解法分析：
如果知道了左子树和右子树的最大深度 l 和 r，那么该二叉树的最大深度即为
```
maxDepth = max(l, r) + 1
```
，所以可以使用递归解决
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
var maxDepth = function(root) {
    if(root === null) {
        return 0;
    } else {
        const left = maxDepth(root.left);
        const right = maxDepth(root.right);
        const maxHeight = Math.max(left, right) + 1;
        return maxHeight;
    }
};
```