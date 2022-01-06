---
title: 二叉树的序列化与反序列化
date: 2021-12-28 15:12:24
tags: [算法, leetcode]
copyright: true
---
题目链接：
https://leetcode-cn.com/problems/serialize-and-deserialize-binary-tree/
解法分析：DFS。选择前序遍历，这样更容易确认根节点的位置。

```js
/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */

/**
 * Encodes a tree to a single string.
 *
 * @param {TreeNode} root
 * @return {string}
 */
var serialize = function(root) {
    if (root === null) {
        return 'N';
    }
    const left = serialize(root.left);
    const right = serialize(root.right);

    // 选择前序遍历，是因为这样在反序列化时更容易定位根节点
    return root.val + ',' + left + ',' +  right;
};

/**
 * Decodes your encoded data to tree.
 *
 * @param {string} data
 * @return {TreeNode}
 */
var deserialize = function(data) {
    const list = data.split(',');
    return dfs(list);
   
    function dfs(list) {
        // 先序遍历，所以第一项就是根
        const rootVal = list.shift(); 
        // 特判
        if (rootVal === 'N') {
            return null;
        }

        // 创建根节点
        const root = new TreeNode(rootVal); 
        root.left = dfs(list);
        root.right = dfs(list);
        return root;
    }
};

/**
 * Your functions will be called as such:
 * deserialize(serialize(root));
 */
```