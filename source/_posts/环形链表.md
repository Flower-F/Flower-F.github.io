---
title: 环形链表
date: 2021-12-02 22:25:49
tags: [算法, codetop]
copyright: true
---
题目链接：
https://leetcode-cn.com/problems/linked-list-cycle
解法分析：快慢双指针最经典的题目
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
 * @return {boolean}
 */
var hasCycle = function(head) {
    if(head === null || head.next === null) {
        return false;
    }
    let slow = head, fast = head.next;
    while(slow !== fast) {
        if(!fast || !fast.next) {
            return false;
        }
        fast = fast.next.next;
        slow = slow.next;
    }
    return true;
};
```