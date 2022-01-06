---
title: 二叉搜索树的第 K 大节点
date: 2021-12-08 20:59:51
tags: [算法, codetop]
copyright: true
---
题目链接：
https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-di-kda-jie-dian-lcof/
解法分析：二叉搜索树的中序遍历是一个递增数组，然后利用这点就可以完成了。

```js
/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */
/**
 * @param {TreeNode} root
 * @param {number} k
 * @return {number}
 */
var kthLargest = function(root, k) {
    if(root === null) {
        return null;
    }
    const res = []; // 存储中序遍历的结果
    inorderReverse(root);
    return res[res.length - k];
    
    function inorderReverse(root) {
        if(root === null) {
            return;
        }
        inorderReverse(root.left);
        res.push(root.val);
        inorderReverse(root.right);
    }
};
```