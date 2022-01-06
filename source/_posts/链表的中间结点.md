---
title: 链表的中间结点
date: 2022-01-06 11:58:54
tags: [算法, leetcode]
copyright: true
---
题目链接：
https://leetcode-cn.com/problems/middle-of-the-linked-list/
解法分析：快慢指针
```js
/**
 * Definition for singly-linked list.
 * function ListNode(val, next) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.next = (next===undefined ? null : next)
 * }
 */
/**
 * @param {ListNode} head
 * @return {ListNode}
 */
var middleNode = function(head) {
    let slow, fast;
    slow = fast = head;
    while (fast && fast.next) {
        slow = slow.next;
        fast = fast.next.next;
    }
    return slow;
};
```