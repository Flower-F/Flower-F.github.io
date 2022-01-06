---
title: 用两个栈实现队列
date: 2021-12-07 21:59:33
tags: [算法, codetop]
copyright: true
---
题目链接：
https://leetcode-cn.com/problems/yong-liang-ge-zhan-shi-xian-dui-lie-lcof/
解法分析：当 stack2 不为空时，直接弹出 stack2 的栈顶元素；当 stack2 为空时，把 stack1 的元素依次弹入 stack2 中

```js
var CQueue = function() {
    this.stack1 = [];
    this.stack2 = [];
};

/** 
 * @param {number} value
 * @return {void}
 */
CQueue.prototype.appendTail = function(value) {
    this.stack1.push(value);
};

/**
 * @return {number}
 */
CQueue.prototype.deleteHead = function() {
    if(this.stack1.length === 0 && this.stack2.length === 0) {
        return -1;
    }
    if(this.stack2.length === 0) {
        while(this.stack1.length > 0) {
            this.stack2.push(this.stack1.pop());
        }
    }
    return this.stack2.pop();
};

/**
 * Your CQueue object will be instantiated and called as such:
 * var obj = new CQueue()
 * obj.appendTail(value)
 * var param_2 = obj.deleteHead()
 */
```