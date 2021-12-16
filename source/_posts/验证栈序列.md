---
title: 验证栈序列
date: 2021-12-15 17:46:01
tags: [算法, leetcode]
copyright: true
---
题目链接：
https://leetcode-cn.com/problems/validate-stack-sequences/
解法分析：将 pushed 中的每个数都 push 到栈中，同时检查这个数是不是 popped 中下一个要 pop 的值，如果是就把它 pop 出来。

```ts
function validateStackSequences(pushed: number[], popped: number[]): boolean {
    const stack: number[] = [];
    let index = 0;

    for (const item of pushed) {
        stack.push(item);
        while(stack.length && stack[stack.length - 1] === popped[index]) {
            stack.pop();
            index++;
        }
    }

    return stack.length === 0;
};
```