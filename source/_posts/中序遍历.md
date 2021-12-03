---
title: 中序遍历
date: 2021-12-03 18:37:35
tags: [算法, codetop]
copyright: true
---
题目链接：
https://leetcode-cn.com/problems/binary-tree-inorder-traversal/
解法分析：纯净的中序遍历
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
 * @return {number[]}
 */
var inorderTraversal = function(root) {
    const res = []
    helper(root);
    return res;
    
    function helper(root) {
        if(root === null) {
            return;
        }
        helper(root.left);
        res.push(root.val);
        helper(root.right);
    }
};
```