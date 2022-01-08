---
title: 旋转链表
date: 2021-12-18 13:15:26
tags: [算法, leetcode]
copyright: true
---
题目链接：
https://leetcode-cn.com/problems/rotate-list/
解法分析：
解法一：直接法。假设链表长度为 n。首先显然需要 k % n，然后只需要将链表的后 k 个节点移动到链表的最前面，然后将链表的后 k 个节点和前 n - k 个节点连接到一块即可。问题就转变成了找到链表的尾节点、头节点（已知）、第 n - k 个节点。

```ts
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     val: number
 *     next: ListNode | null
 *     constructor(val?: number, next?: ListNode | null) {
 *         this.val = (val===undefined ? 0 : val)
 *         this.next = (next===undefined ? null : next)
 *     }
 * }
 */

function rotateRight(head: ListNode | null, k: number): ListNode | null {
    if (head === null || k === 0) {
        // 这里要返回 head，不能返回 null
        // return null
        return head;
    }
    let n = 0; // 链表长度
    let tail = null; // 存储尾节点
    for (let p = head; p !== null; p = p.next) {
        n++;
        tail = p;
    }
    k %= n;
    let nk = head; // 第 n - k 个节点
    for (let i = 0; i < n - k - 1; i++) {
        nk = nk.next;
    }
    tail.next = head;
    head = nk.next;
    nk.next = null;

    return head;
};
```

解法二：
首先思考一下如何返回链表倒数第 k 个节点。
- 采用快慢指针
- 快指针与慢指针都以每步一个节点的速度向后遍历
- 快指针比慢指针先走 k 步
- 当快指针到达终点时，慢指针正好是倒数第 k 个节点

```js
let slow = (fast = head);
while (fast.next) {
  if (k-- <= 0) {
    slow = slow.next;
  }
  fast = fast.next;
}
```
所以解法如下：
```js
var rotateRight = function (head, k) {
    if (!head || !head.next) return head;
    let count = 0,
        now = head;
    while (now) {
        now = now.next;
        count++;
    }
    k = k % count;
    let slow = (fast = head);
    while (fast.next) {
        if (k-- <= 0) {
            slow = slow.next;
        }
        fast = fast.next;
    }
    fast.next = head;
    let res = slow.next;
    slow.next = null;
    return res;
};
```



参考题解：
1. https://leetcode-cn.com/problems/rotate-list/solution/xuan-zhuan-lian-biao-tu-jie-lian-biao-zu-ku33/
2. 公众号力扣加加