---
title: 相交链表
date: 2021-12-05 22:15:11
tags: [算法, codetop]
copyright: true
---
题目链接：
https://leetcode-cn.com/problems/intersection-of-two-linked-lists/
解法分析：
- 如果指针 pA 不为空，则将指针 A 移到下一个节点；如果指针 pB 不为空，则将指针 pB 移到下一个节点。
- 如果指针 pA 为空，则将指针 pA 移到链表 headB 的头节点；如果指针 pB 为空，则将指针 pB 移到链表 headA 的头节点。

```js
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */

/**
 * @param {ListNode} headA
 * @param {ListNode} headB
 * @return {ListNode}
 */
var getIntersectionNode = function(headA, headB) {
    if(headA === null || headB === null) {
        return null;
    }
    
    let p1 = headA, p2 = headB;
    while(p1 !== p2) {
        p1 = p1 ? p1.next : headB;
        p2 = p2 ? p2.next : headA;
    }
    return p1;
};
```