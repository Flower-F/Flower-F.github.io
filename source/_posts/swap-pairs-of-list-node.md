---
title: 两两交换链表中的节点
date: 2021-12-19 19:24:39
tags: [算法, leetcode]
copyright: true
---
题目链接：
https://leetcode-cn.com/problems/swap-nodes-in-pairs/
解法分析：具体思路见代码注释，主要难点是指针变换的顺序问题，还有灵活设定 preHead 节点，使得边界的判断变得更简单。

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
var swapPairs = function(head) {
    if (!head || !head.next) {
        return head;
    }
    const res = head.next; // 答案
    let preNode = new ListNode(-1);
    preNode.next = head;

    let cur = head; 
    // A.next = B.next;
    // B.next = A;
    // preA.next = B;
    while (cur && cur.next) {
        const B = cur.next;
        const nextB = B.next;
        // A 节点的 next 指向 nextB
        cur.next = nextB;
        // B 节点的 next 指向 A
        B.next = cur;
        // preA 节点的 next 指向 B
        preNode.next = B;
        // 修改 preNode
        preNode = cur;
        // 移动 cur
        cur = nextB;
    }
    return res;
};
```

参考题解：公众号力扣加加