---
title: 链表中的倒数第n个节点
date: 2021-12-03 18:42:26
tags: [算法, codetop]
copyright: true
---
题目链接：
https://leetcode-cn.com/problems/lian-biao-zhong-dao-shu-di-kge-jie-dian-lcof/
解法分析：
快慢指针的经典问题。将fast 指向链表的第 k + 1 个节点，slow 指向链表的第一个节点，此时指针 fast 与 slow 二者之间刚好间隔 k 个节点。此时两个指针同步向后走，当 fast 走到链表的尾部空节点时，则此时 slow 指针刚好指向链表的倒数第 k 个节点。

```js
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */
/**
 * @param {ListNode} head
 * @param {number} k
 * @return {ListNode}
 */
var getKthFromEnd = function(head, k) {
    let slow, fast;
    slow = fast = head;
    for(let i = 0; i < k; i++) {
        fast = fast.next;
    }
    while(fast) {
        fast = fast.next;
        slow = slow.next;
    }
    return slow;
};
```