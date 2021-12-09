---
title: 平衡二叉树
date: 2021-12-09 17:54:51
tags: [算法, codetop]
copyright: true
---
题目链接：
https://leetcode-cn.com/problems/balanced-binary-tree/
解法分析：自底向上递归。对二叉树进行前序遍历，自底向上返回子树的最大高度，若某子树不是平衡树，则直接返回
递归返回值：
- 当节点 root 的左右子树的高度差小于 2，返回以 root 为根节点的子树的最大高度，即 max(left, right) + 1
- 当节点 root 的左右子树的高度差大于等于 2，返回 -1，表示该树不是平衡树

```ts
/**
 * Definition for a binary tree node.
 * class TreeNode {
 *     val: number
 *     left: TreeNode | null
 *     right: TreeNode | null
 *     constructor(val?: number, left?: TreeNode | null, right?: TreeNode | null) {
 *         this.val = (val===undefined ? 0 : val)
 *         this.left = (left===undefined ? null : left)
 *         this.right = (right===undefined ? null : right)
 *     }
 * }
 */

function isBalanced(root: TreeNode | null): boolean {
    return helper(root) !== -1;
    
    function helper(root: TreeNode | null): number {
        if(root === null) {
            return 0;
        }
        const left = helper(root.left);
        if(left === -1) {
            return -1;
        }
        const right = helper(root.right);
        if(right === -1) {
            return -1;
        }
        return Math.abs(left - right) < 2 ? Math.max(left, right) + 1 : -1;
    }
};
```

参考题解：
作者：jyd
链接：
https://leetcode-cn.com/problems/balanced-binary-tree/solution/balanced-binary-tree-di-gui-fang-fa-by-jin40789108/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。