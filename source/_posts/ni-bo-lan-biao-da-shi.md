---
title: 逆波兰表达式求值
date: 2021-12-14 15:26:34
tags: [算法, leetcode]
copyright: true
---
题目链接：
https://leetcode-cn.com/problems/evaluate-reverse-polish-notation/

解法分析：
逆波兰表达式即后缀表达式。
- 如果遇到操作数，则将操作数入栈；
- 如果遇到运算符，则将两个操作数出栈，其中先出栈的是右操作数，后出栈的是左操作数，使用运算符对两个操作数进行运算，将运算得到的新操作数入栈。

```ts
function evalRPN(tokens: string[]): number {
    let ans = 0;
    const stack: number[] = [];
    for(const token of tokens) {
        if(token ===  '+' || token === '-' || token ===  '*' || token === '/') {
            const right = stack.pop();
            const left = stack.pop();

            switch(token) {
                case '+':
                    ans = left + right;
                    break;
                case '-':
                    ans = left - right;
                    break;
                case '*':
                    ans = left * right;
                    break;
                case '/':
                    // 这里注意要 | 0，否则会出错，因为 left / right 可能是 NaN
                    ans = Math.floor(left / right | 0);
                    break;
            }

            stack.push(ans);
        } else {
            stack.push(parseInt(token));
        }
    }

    return stack.pop();
};
```

参考题解：
https://leetcode-cn.com/problems/evaluate-reverse-polish-notation/solution/ni-bo-lan-biao-da-shi-qiu-zhi-by-leetcod-wue9/