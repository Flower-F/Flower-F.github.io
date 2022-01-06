---
title: 前序遍历
date: 2021-12-03 18:49:32
tags: [算法, codetop]
copyright: true
---
题目链接：
https://leetcode-cn.com/problems/binary-tree-preorder-traversal/
解法分析：纯净的前序遍历
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
var preorderTraversal = function(root) {
    const res = [];
    helper(root);
    return res;
    
    function helper(root) {
        if(root === null) {
            return;
        }
        res.push(root.val);
        helper(root.left);
        helper(root.right);
    }
};
```