---
title: 二叉树展开为链表
date: 2022-01-13 19:29:33
tags: [leetcode, 算法]
copyright: true
---
题目链接：
https://leetcode-cn.com/problems/flatten-binary-tree-to-linked-list/
解法分析：

解法一：前序遍历

![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/20220113194451.png)
由图示可知，树扯直以后特征是：每个左孩子为空，右孩子为前序遍历的下一位

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
 * @return {void} Do not return anything, modify root in-place instead.
 */
var flatten = function(root) {
    const list = [];
    preorderTraversal(root);

    for (let i = 1; i < list.length; i++) {
        const pre = list[i - 1], cur = list[i];
        pre.left = null;
        pre.right = cur;
    }

    function preorderTraversal(root) {
        if (!root) return;

        list.push(root);
        preorderTraversal(root.left);
        preorderTraversal(root.right);
    }
};
```

解法二：递归

![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/20220113195358.png)

把树拉直的步骤可归为以下三步：
- 将左子树和右子树扯直
![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/20220113195524.png)
- 将右子树换成左子树
![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/20220113200803.png)
- 将原来的右子树接到当前右子树的后面
![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/20220113200913.png)

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
 * @return {void} Do not return anything, modify root in-place instead.
 */
var flatten = function(root) {
    if (!root) {
        return;
    }

    flatten(root.left);
    flatten(root.right);

    // 后序遍历，所以此时二叉树已经被拉平
    const leftTree = root.left, rightTree = root.right;

    // 将右子树换成左子树
    root.right = leftTree;
    root.left = null;

    //将原来的右子树接到当前右子树的后面
    let p = root;
    while (p.right) { // 查找最右节点
        p = p.right;
    }

    p.right = rightTree;
};
```

参考题解：
1. https://labuladong.gitee.io/algo/2/17/21/
2. https://leetcode-cn.com/problems/flatten-binary-tree-to-linked-list/solution/er-cha-shu-zhan-kai-wei-lian-biao-by-leetcode-solu/