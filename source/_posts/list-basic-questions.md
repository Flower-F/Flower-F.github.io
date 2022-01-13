---
title: 链表基础六大题型
date: 2022-01-13 16:17:28
tags: [javascript]
copyright: true
---
- [合并两个有序链表](https://leetcode-cn.com/problems/merge-two-sorted-lists/)
- [链表中倒数第 K 个节点](https://leetcode-cn.com/problems/lian-biao-zhong-dao-shu-di-kge-jie-dian-lcof/) & [删除链表的倒数第 N 个结点](https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/)
- [链表的中间结点](https://leetcode-cn.com/problems/middle-of-the-linked-list/)
- [环形链表](https://leetcode-cn.com/problems/linked-list-cycle/) & [环形链表 II](https://leetcode-cn.com/problems/linked-list-cycle-ii/)
- [相交链表](https://leetcode-cn.com/problems/intersection-of-two-linked-lists/)
- [反转链表](https://leetcode-cn.com/problems/reverse-linked-list/) & [反转链表 II](https://leetcode-cn.com/problems/reverse-linked-list-ii/)

# 合并两个有序链表
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
    let dummyHead = new ListNode(-1);
    let p1 = list1, p2 = list2;

    let p = dummyHead;
    while (p1 && p2) {
        if (p1.val > p2.val) {
            p.next = p2;
            p2 = p2.next;
        } else {
            p.next = p1;
            p1 = p1.next;
        }
        p = p.next;
    }

    p.next = p1 || p2;

    return dummyHead.next;
};
```

# 链表中倒数第 K 个节点
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

    for (let i = 0; i < k; i++) {
        fast = fast.next;
    }

    while (fast) {
        fast = fast.next;
        slow = slow.next;
    }

    return slow;
};
```

# 删除链表的倒数第 N 个结点
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
 * @param {number} n
 * @return {ListNode}
 */
var removeNthFromEnd = function(head, n) {
    const dummyHead = new ListNode(-1, head);
    // 要删除倒数第 n 个节点，则需要找到倒数第 n + 1 个节点
    const temp = getKthFromEnd(dummyHead, n + 1);
    temp.next = temp.next.next;

    return dummyHead.next;

    // 返回链表的倒数第 k 个节点
    function getKthFromEnd(head, k) {
        let slow, fast;
        slow = fast = head;
        for (let i = 0; i < k; i++) {
            fast = fast.next;
        }
        while (fast) {
            fast = fast.next;
            slow = slow.next;
        }
        return slow;
    }
};
```

# 链表的中间结点
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
        fast = fast.next.next;
        slow = slow.next;
    }
    return slow;
};
```

# 环形链表
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
    let slow, fast;
    slow = fast = head;

    while (fast && fast.next) {
        fast = fast.next.next;
        slow = slow.next;

        if (fast === slow) {
            return true;
        }
    }

    return false;
};
```

# 环形链表 II
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

    let flag = false; // 标记是否有环
    while (fast && fast.next) {
        slow = slow.next;
        fast = fast.next.next;

        if (slow === fast) {
            flag = true;
            break;
        }
    }

    if (!flag) {
        return null;
    }

    slow = head;
    while (slow !== fast) {
        slow = slow.next;
        fast = fast.next;
    }

    return slow;
};
```

# 相交链表
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
    let p1 = headA, p2 = headB;
    while (p1 !== p2) {
        p1 = p1 ? p1.next : headB;
        p2 = p2 ? p2.next : headA;
    } 
    return p1;
};
```

# 反转链表

## 迭代版
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
    let prev = null, cur = head;
    while (cur) {
        const tmp = cur.next; // 存储 cur 的下一个节点
        cur.next = prev;
        // 移动指针，继续前进
        prev = cur;
        cur = tmp;
    }
    return prev; // prev 最后指向的就是头节点
};
```

## 递归版
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
    // 若链表只有一个节点或根本没有节点，直接返回即可
    if (head === null || head.next === null) {
        return head;
    }

    const newList = reverseList(head.next);
    // 假设节点 head 为节点 1，节点 head 的下一个节点为节点 2
    const p2 = head.next; // 获取节点 2
    p2.next = head; // 节点 2 的 next 指向节点 1
    head.next = null; // 节点 1 的 next 设为空

    return newList;
};
```

![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/20220113161308.png)

如果对 2->3->4 进行递归反转，则得到：

![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/20220113161403.png)

接下来只要把节点 2 的 next 指向节点 1，然后节点 1 的 next 指向 null 即可，如图：

![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/20220113161457.png)


# 反转链表 II

- 定义两个指针，分别称之为 g（guard 守卫） 和 p（point）。我们首先根据方法的参数 left 确定 g 和 p 的位置。将 g 移动到第一个要反转的节点的前面，将 p 移动到第一个要反转的节点的位置上。我们以 left = 2，right = 4 为例。
- 头插法：将 p 后面的元素删除，然后添加到 g 的后面。
- 重复上面头插法的步骤，一共需要操作 right - left 次。

1. 
![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/20220113171031.png)

2. 
![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/20220113171056.png)

3. 
![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/20220113171126.png)

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
 * @param {number} left
 * @param {number} right
 * @return {ListNode}
 */
var reverseBetween = function(head, left, right) {
    let dummyHead = new ListNode(-1, head);

    let g = dummyHead, p = dummyHead.next;

    for (let i = 0; i < left - 1; i++) {
        g = g.next;
        p = p.next;
    }

    for (let i = 0; i < right - left; i++) {
        // 暂存删除节点
        let removed = p.next;
        // 删除节点
        p.next = p.next.next;
        // 插入刚才删除的节点
        removed.next = g.next;
        g.next = removed;
    }

    return dummyHead.next;
};
```

参考资料：
1. https://www.pianshen.com/article/1931399442/
2. https://labuladong.gitee.io/algo/2/16/15/
3. https://leetcode-cn.com/problems/reverse-linked-list-ii/solution/java-shuang-zhi-zhen-tou-cha-fa-by-mu-yi-cheng-zho/