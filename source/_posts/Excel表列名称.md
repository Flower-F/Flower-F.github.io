---
title: Excel 表列名称
date: 2021-12-09 18:34:56
tags: [算法, codetop]
copyright: true
---
题目链接：
https://leetcode-cn.com/problems/excel-sheet-column-title/
解法分析：转换思路。和正常 0 ~ 25 的 26 进制相比，本质上就是每一位多加了 1。假设 A == 0，B == 1，那么 AB = 26 * 0 + 1 * 1，而现在 AB = 26 * (0 + 1) + 1 * (1 + 1)，所以只要在处理每一位的时候减 1，就可以按照正常的 26 进制来处理

```ts
function convertToTitle(columnNumber: number): string {
    const res = [];
    while(columnNumber) {
        // 减去 1，变成正常的 26 进制
        columnNumber--;
        const temp = String.fromCharCode((columnNumber % 26) + 'A'.charCodeAt(0));
        res.push(temp);
        columnNumber = Math.floor(columnNumber / 26);
    }
    return res.reverse().join('');
};
```

参考题解：
作者：LeetCode-Solution
链接：
https://leetcode-cn.com/problems/excel-sheet-column-title/solution/excelbiao-lie-ming-cheng-by-leetcode-sol-hgj4/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。