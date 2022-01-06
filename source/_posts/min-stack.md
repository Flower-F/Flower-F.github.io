---
title: 最小栈
date: 2021-12-05 22:29:12
tags: [算法, codetop]
copyright: true
---
题目链接：
https://leetcode-cn.com/problems/min-stack/
解法分析：维护两个栈，一个最小值的栈，一个存储所有数据的栈，注意当栈为空时直接入栈。

```js
var MinStack = function() {
    this.dataStack = [];
    this.minStack = [];
};

/** 
 * @param {number} val
 * @return {void}
 */
MinStack.prototype.push = function(val) {
    if(this.dataStack.length === 0) {
        this.dataStack.push(val);
        this.minStack.push(val);
        return;
    }
    if(val <= this.getMin()) {
        this.minStack.push(val);
    }
    this.dataStack.push(val);
};

/**
 * @return {void}
 */
MinStack.prototype.pop = function() {
    const minVal = this.getMin(), topVal = this.top();
    if(minVal === topVal) {
        this.minStack.pop();
    }
    this.dataStack.pop();
};

/**
 * @return {number}
 */
MinStack.prototype.top = function() {
    return this.dataStack[this.dataStack.length - 1];
};

/**
 * @return {number}
 */
MinStack.prototype.getMin = function() {
    return this.minStack[this.minStack.length - 1];
};

/**
 * Your MinStack object will be instantiated and called as such:
 * var obj = new MinStack()
 * obj.push(val)
 * obj.pop()
 * var param_3 = obj.top()
 * var param_4 = obj.getMin()
 */
```