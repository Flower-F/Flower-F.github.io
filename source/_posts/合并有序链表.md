---
title: 合并有序链表
date: 2021-12-05 21:48:06
tags: [算法, codetop]
copyright: true
---
题目链接：
https://leetcode-cn.com/problems/merge-two-sorted-lists/
解法分析：使用非递归的方法进行遍历即可，最后再合并还未比较的部分

```js
/**
 * Definition for singly-linked list.
 * function ListNode(val, next) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.next = (next===undefined ? null : next)
 * }
 */
/**
 * @param {ListNode} list1
 * @param {ListNode} list2
 * @return {ListNode}
 */
var mergeTwoLists = function(list1, list2) {
    let p1 = list1, p2 = list2;
    const dummy = new ListNode(-1);
    let cur = dummy;
    
    while(p1 && p2) {
        const val1 = p1.val, val2 = p2.val;
        if(val1 >= val2) {
            cur.next = p2;
            p2 = p2.next;
        } else {
            cur.next = p1;
            p1 = p1.next;
        }
        cur = cur.next;
    }
    
    cur.next = p1 ? p1 : p2;
    
    return dummy.next;
};
```