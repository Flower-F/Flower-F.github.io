---
title: 圆圈中最后剩下的数字
date: 2021-12-06 23:20:56
tags: [算法, codetop]
copyright: true
---
题目链接：
https://leetcode-cn.com/problems/yuan-quan-zhong-zui-hou-sheng-xia-de-shu-zi-lcof/
解法分析：约瑟夫环问题

```js
/**
 * @param {number} n
 * @param {number} m
 * @return {number}
 */
var lastRemaining = function(n, m) {
    let x = 0;
    for(let i = 2; i <= n; i++) {
        x = (x + m) % i;
    }
    return x;
};
```

参考题解：
作者：jyd
链接：
https://leetcode-cn.com/problems/yuan-quan-zhong-zui-hou-sheng-xia-de-shu-zi-lcof/solution/jian-zhi-offer-62-yuan-quan-zhong-zui-ho-dcow/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。