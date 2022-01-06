---
title: 环形链表 II
date: 2021-12-22 10:51:18
tags: [算法, leetcode]
copyright: true
---
题目链接：
https://leetcode-cn.com/problems/linked-list-cycle-ii/
解法分析：使用快慢指针，可以证明如果链表存在环，则快慢指针最终将会相遇。

设链表共有 a + b 个节点，其中链表头部到链表入口 有 a 个节点（不计链表入口节点），链表环有 b 个节点。
根据：
f = 2s（fast 走的步数是 slow 步数的 2 倍）
f = s + nb (fast 比 slow 多走了 n 个环的长度）
推出：
f = 2nb，s = nb （fast 和 slow 分别走了 2n，n 个环的周长）

从 head 结点走到入环点需要走：a + nb， 而 slow 已经走了 nb，那么 slow 再走 a 步就是入环点了。
如何知道 slow 刚好走了 a 步？fast 从 head 开始，和 slow 指针一起走，相遇时刚好就是 a 步。

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
 * @return {ListNode}
 */
var detectCycle = function(head) {
    let slow, fast;
    slow = fast = head;
    while (fast && fast.next) {
        fast = fast.next.next;
        slow = slow.next;

        // 若存在环
        if (slow === fast) {
            break;
        }
    }

    // 边界特判
    if (fast === null || fast.next === null) {
        return null;
    }

    // 寻找节点
    fast = head;
    while (slow !== fast) {
        slow = slow.next;
        fast = fast.next;
    }

    return fast;
};
```

参考题解：
作者：jyd
链接：
https://leetcode-cn.com/problems/linked-list-cycle-ii/solution/linked-list-cycle-ii-kuai-man-zhi-zhen-shuang-zhi-/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。