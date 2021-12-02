---
title: 反转链表
date: 2021-12-02 17:39:37
tags: [算法, codetop]
copyright: true
---
题目链接：
https://leetcode-cn.com/problems/reverse-linked-list/
在遍历链表时，将当前节点的 next 指针改为指向前一个节点。由于节点没有引用其前一个节点，因此必须事先存储其前一个节点。在更改引用之前，还需要存储后一个节点。最后返回新的头引用。

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
var reverseList = function(head) {
    let pre = null, cur = head;
    while(cur) {
        const next = cur.next; // 存储下一节点
        cur.next = pre; // 下一个左移
        pre = cur; // 前一个左移
        cur = next; // 当前节点前进
    }
    return pre; // 返回头节点
};
```

题解来源：
作者：LeetCode-Solution
链接：
https://leetcode-cn.com/problems/reverse-linked-list/solution/fan-zhuan-lian-biao-by-leetcode-solution-d1k2/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

