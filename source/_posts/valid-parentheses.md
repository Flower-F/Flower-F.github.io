---
title: 有效的括号
date: 2021-12-02 20:32:49
tags: [算法, codetop]
copyright: true
---
题目链接：
https://leetcode-cn.com/problems/valid-parentheses
解法分析：leetcode 里面最经典的栈应用
```js
/**
 * @param {string} s
 * @return {boolean}
 */

// 栈
var isValid = function(s) {
    if(s.length % 2 === 1) {
        return false;
    }
    const stack = [];
    for(let i = 0; i < s.length; i++) {
        const c = s[i];
        if(c === '(' || c === '[' || c ==='{') {
            stack.push(c);
        } else {
            const top = stack[stack.length - 1];
            if(top === '(' && c === ')' ||
                top === '[' && c === ']' ||
                top === '{' && c === '}') {
                stack.pop();
            } else {
                return false;
            }
        }
    }
    return stack.length === 0;
};
```