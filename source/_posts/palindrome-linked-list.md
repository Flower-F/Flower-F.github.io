---
title: 回文链表
date: 2022-01-13 17:38:19
tags: [leetcode, 算法]
copyright: true
---
题目链接：
https://leetcode-cn.com/problems/palindrome-linked-list/
解法分析：相当于是综合[链表的中间结点](https://leetcode-cn.com/problems/middle-of-the-linked-list/)和[反转链表](https://leetcode-cn.com/problems/reverse-linked-list/)

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
 * @return {boolean}
 */
var isPalindrome = function(head) {
    let slow, fast;
    slow = fast = head;

    while (fast && fast.next) {
        fast = fast.next.next;
        slow = slow.next;
    }

    // 如果 fast 没有指向 null，说明链表长度为奇数，slow 还要再前进一步
    if (fast) {
        slow = slow.next;
    }

    let left = head, right = reverseList(slow);
    while (right) {
        if (left.val !== right.val) {
            return false;
        }
        left = left.next;
        right = right.next;
    }

    return true;

    function reverseList(head) {
        let pre = null, cur = head;
        while (cur) {
            const tmp = cur.next;
            cur.next = pre;
            pre = cur;
            cur = tmp;
        }
        return pre;
    }
};
```