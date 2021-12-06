---
title: 翻转二叉树
date: 2021-12-06 23:30:32
tags: [算法, codetop]
copyright: true
---
题目链接：
https://leetcode-cn.com/problems/invert-binary-tree/
解法分析：从根节点开始，递归地对树进行遍历，并从叶子节点先开始翻转。如果当前遍历到的节点 root 的左右两棵子树都已经翻转，那么我们只需要交换两棵子树的位置，即可完成以 root 为根节点的整棵子树的翻转。

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
 * @return {TreeNode}
 */
var invertTree = function(root) {
    if(!root) {
        return null;
    }
    // 将当前节点的左右子树交换
    const left = root.left;
    const right = root.right;
    root.right = left;
    root.left = right;
    // 递归交换当前节点的左子树和右子树
    invertTree(root.left);
    invertTree(root.right);
    // 函数返回时就表示当前这个节点，以及它的左右子树都交换完毕
    return root;
};
```