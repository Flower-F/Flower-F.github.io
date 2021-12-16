---
title: 基本计算器 II
date: 2021-12-15 20:38:17
tags: [算法, leetcode]
copyright: true
---
题目链接：
https://leetcode-cn.com/problems/basic-calculator-ii/
解法分析：
- 从左到右遍历 s
- 如果是数字，则更新数字
- 如果是空格，则跳过
- 如果是运算符，则按照运算符规则计算，并将计算结果重新入栈，具体见代码。

```ts
function calculate(s: string): number {
    const stack: number[] = [];
    s += '$'; // 哨兵，为了让最后一个符号可以进入。举例来说，如果不加入哨兵，3+2*2 会被识别为 3+2。
    let prev = '+';
    let num = 0;

    for (const ch of s) {
        if(!isNaN(+ch) && ch !== ' ') {
            // 这里判断一定要注意 isNaN 判断空格也是 false，另外 +" " 的结果为 0
            num = num * 10 + parseInt(ch);
        } else if (ch === ' ') {
            continue;
        } else {
            switch (prev) {
                case '+':
                    stack.push(num);
                    break;
                case '-':
                    stack.push(-num);
                    break;
                case '*':
                    stack.push(stack.pop() * num);
                    break;
                case '/':
                    stack.push(Math.floor(stack.pop() / num | 0));
                    break;
            }
            prev = ch;
            num = 0;
        }
    }

    const sum = stack.reduce((a, b) => (a + b));
    return sum;
};
```

参考题解：
https://mp.weixin.qq.com/s?__biz=MzI4MzUxNjI3OA==&mid=2247486874&idx=2&sn=3f42546c132983bf22828a99b1c6e7b4&chksm=eb88c183dcff48956d97d1b67e8d070b9561be26f66006773d153457494ca8c43db73a8e7343&token=1469603194&lang=zh_CN#rd