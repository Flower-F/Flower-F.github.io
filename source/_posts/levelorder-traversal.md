---
title: 层序遍历
date: 2022-01-08 12:43:52
tags: [算法, nowcoder]
copyright: true
---
题目链接：
https://www.nowcoder.com/practice/04a5560e43e24e9db4595865dc9c63a3
解法分析：纯的层序遍历，使用 BFS

```js
/*
 * function TreeNode(x) {
 *   this.val = x;
 *   this.left = null;
 *   this.right = null;
 * }
 */

/**
  * 
  * @param root TreeNode类 
  * @return int整型二维数组
  */
function levelOrder(root) {
    // write code here
    if (!root) {
        return [];
    }
    const res = [], queue = [];
    
    queue.push(root);
    while (queue.length) {
        const currentLevelSize = queue.length;
        res.push([]);
        for (let i = 0; i < currentLevelSize; i++) {
            const node = queue.shift();
            res[res.length - 1].push(node.val);
            node.left && queue.push(node.left);
            node.right && queue.push(node.right);
        }
    }
    
    return res;
}

module.exports = {
    levelOrder : levelOrder
};
```