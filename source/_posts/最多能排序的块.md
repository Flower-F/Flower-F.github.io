---
title: 最多能排序的块
date: 2021-12-17 10:29:39
tags: [算法, leetcode]
copyright: true
---
题目链接：
https://leetcode-cn.com/problems/max-chunks-to-make-sorted-ii/
解法分析：
使用单调栈。比如 [2,1,3,4,4]，遍历到 1 的时候会发现 1 比 2 小，因此 2， 1 需要在一块，我们可以将 2 和 1 融合，并重新压回栈。那么融合成 1 还是 2 呢？答案是 2，因为 2 是瓶颈，这提示我们可以用一个递增栈来完成。
因此本质上栈存储的每一个元素就代表一个块，而栈里面的每一个元素的值就是块的最大值。
以 [2,1,3,4,4] 来说， stack 的变化过程大概是：
- [2] 
- 1 被融合了，保持 [2] 不变
- [2,3]
- [2,3,4]
- [2,3,4,4]

简单来说，就是**将栈中每一个减序列压缩合并成该序列的最大的值**。 因此最终返回 stack 的长度就可以了。

```ts
function maxChunksToSorted(arr: number[]): number {
    const stack: number[] = [];
    for (const num of arr) {
        if (stack.length && num < stack[stack.length - 1]) {
            const top = stack[stack.length - 1];
            while (stack.length && num < stack[stack.length - 1]) {
                stack.pop();
            }
            stack.push(top);
        } else {
            stack.push(num);
        }
    }
    return stack.length;
};
```

参考题解：
公众号力扣加加