---
title: 删除有序数组的重复元素
date: 2022-01-13 10:25:10
tags: [算法, leetcode]
copyright: true
---
该类题目包含以下的四道题：
- [删除有序数组中的重复项](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array/)
- [删除排序链表中的重复元素](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list/)
- [移除元素](https://leetcode-cn.com/problems/remove-element/)
- [移动零](https://leetcode-cn.com/problems/move-zeroes/)

# 删除有序数组中的重复项
```js
/**
 * @param {number[]} nums
 * @return {number}
 */
var removeDuplicates = function (nums) {
    if (nums.length === 0) {
        return;
    }

    let slow, fast;
    slow = fast = 0;
    const len = nums.length;
    while (fast < len) {
        if (nums[fast] !== nums[slow]) {
            // 每次找到一个不重复的元素就告诉 slow 并让 slow 前进一步
            // 然后给 slow 赋值 fast 所在位置的值
            // 因为第一个元素必定不是重复元素，所以先移动 slow
            slow++;
            nums[slow] = nums[fast];
        }
        fast++;
    }

    return slow + 1;
};
```

# 删除排序链表中的重复元素
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
var deleteDuplicates = function(head) {
    if (!head) {
        return head;
    }
    let slow, fast;
    slow = fast = head;
    while (fast) {
        if (fast.val !== slow.val) {
            slow.next = fast;
            slow = slow.next;
        }
        fast = fast.next;
    }
    // 与后面断开连接
    slow.next = null;
    return head;
};
```

# 移除元素
```js
/**
 * @param {number[]} nums
 * @param {number} val
 * @return {number}
 */
var removeElement = function(nums, val) {
    let slow, fast;
    slow = fast = 0;
    while (fast < nums.length) {
        if (nums[fast] !== val) {
            // 第一个元素也可能等于 val，所以先赋值再移动 slow
            nums[slow] = nums[fast];
            slow++;
        }
        fast++;
    }
    return slow;
};
```

# 移动零
```js
/**
 * @param {number[]} nums
 * @return {void} Do not return anything, modify nums in-place instead.
 */
var moveZeroes = function(nums) {
    let slow, fast;
    slow = fast = 0;
    while (fast < nums.length) {
        if (nums[fast] !== 0) {
            [nums[slow], nums[fast]] = [nums[fast], nums[slow]];
            slow++;
        }
        fast++;
    }
};
```

参考题解：
1. https://labuladong.gitee.io/algo/4/29/120/
2. 代码随想录