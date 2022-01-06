---
title: 基本计算器
date: 2021-12-15 20:42:11
tags: [算法, leetcode]
copyright: true
---
题目链接：
https://leetcode-cn.com/problems/basic-calculator/
解法分析：
维护一个栈 sign，其中栈顶元素记录了当前位置所处的每个括号所「共同形成」的符号。例如，对于字符串 1+2+(3-(4+5))：

- 扫描到 1+2 时，由于当前位置没有被任何括号所包含，则栈顶元素为初始值 +1；
- 扫描到 1+2+(3 时，当前位置被一个括号所包含，该括号前面的符号为 + 号，因此栈顶元素依然 +1；
- 扫描到 1+2+(3-(4 时，当前位置被两个括号所包含，分别对应着 + 号和 - 号，由于 + 号和 - 号合并的结果为 - 号，因此栈顶元素变为 -1。

在得到栈 sign 之后， op 的取值就能够确定了：如果当前遇到了 + 号，则更新 op = sign.top()；如果遇到了 - 号，则更新 op = -sign.top()。
然后，每当遇到 ( 时，都要将当前的 op 取值压入栈中；每当遇到 ) 时，都从栈中弹出一个元素。这样，我们能够在扫描字符串的时候，即时地更新 sign 中的元素。

```ts
function calculate(s: string): number {
    const sign:number[] = []; // 存储正负号
    let ans = 0, num = 0, op = 1;
    sign.push(op);

    for(const ch of s) {
        if (ch === ' ') {
            continue;
        }
        else if (!isNaN(+ch)) {
            num = num * 10 + parseInt(ch);
        } else {
            ans += op * num;
            num = 0;

            switch (ch) {
                case '+':
                    op = sign[sign.length - 1];
                    break;
                case '-':
                    op = -sign[sign.length - 1];
                    break;
                case '(':
                    // 将括号前符号放入栈顶
                    sign.push(op);
                    break;
                case ')':
                    // 将栈顶的符号弹出
                    sign.pop();
                    break;
            }
        }
    }

    return ans + op * num;
};
```

参考题解：
作者：LeetCode-Solution
链接：
https://leetcode-cn.com/problems/basic-calculator/solution/ji-ben-ji-suan-qi-by-leetcode-solution-jvir/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。