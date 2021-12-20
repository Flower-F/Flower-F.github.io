---
title: 有序链表转换二叉搜索树
date: 2021-12-20 11:14:12
tags: [算法, leetcode]
copyright: true
---
题目链接：
https://leetcode-cn.com/problems/convert-sorted-list-to-binary-search-tree/
解法分析：二叉搜索树的任意一个节点，当前节点的值必然大于所有左子树节点的值，且必然小于所有右子树节点的值。因此可以使用如下算法：
- 获取当前链表的中点
- 以链表中点为根
- 中点左边的值都小于它,可以构造左子树
- 同理构造右子树
- 循环第一步

获取链表的中点，可以使用快慢指针解决。
- 定义一个快指针每步前进两个节点，一个慢指针每步前进一个节点
- 当快指针到达尾部的时候，正好慢指针所到的点为中点

```js
/**
 * Definition for singly-linked list.
 * function ListNode(val, next) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.next = (next===undefined ? null : next)
 * }
 */
/**
 * Definition for a binary tree node.
 * function TreeNode(val, left, right) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.left = (left===undefined ? null : left)
 *     this.right = (right===undefined ? null : right)
 * }
 */
/**
 * @param {ListNode} head
 * @return {TreeNode}
 */
var sortedListToBST = function(head) {
    if (!head) {
        return null;
    }
    return dfs(head, null);

    function dfs(head, tail) {
        if (head === tail) {
            return null;
        }

        // 查找链表中点
        let slow, fast;
        slow = fast = head;
        while (fast !== tail && fast.next !== tail) {
            fast = fast.next.next;
            slow = slow.next;
        }
        // 此时慢指针即为中点
        const root = new TreeNode(slow.val);

        // 递归构建左子树
        root.left = dfs(head, slow);
        // 右子树
        root.right = dfs(slow.next, tail);

        return root;
    }
};
```

参考题解：公众号力扣加加