---
title: 矩形重叠
date: 2021-12-11 15:50:17
tags: [算法, codetop]
copyright: true
---
题目链接：
https://leetcode-cn.com/problems/rectangle-overlap/
解法分析：**两个互相重叠的矩形，它们在 x 轴和 y  轴上投影出的区间也是互相重叠的**。这样，我们就将矩形重叠问题转化成了区间重叠问题。

![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/152542.jpg)

矩形重叠是二维的问题，所以情况很多，比较复杂。为了简化问题，我们可以考虑将二维问题转化为一维问题。既然题目中的矩形都是平行于坐标轴的，我们将矩形投影到坐标轴上：

![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/153815.jpg)



区间不重叠的情况显然只有两种：

![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/154203.jpg)


根据图示分析，可知两个矩形 x 方向不重叠的条件是：`x2 <= x3 || x4 <= x1`，同理 y 方向不重叠的条件是：`y2 <= y3 || y4 <= y1`

```js
/**
 * @param {number[]} rec1
 * @param {number[]} rec2
 * @return {boolean}
 */
var isRectangleOverlap = function(rec1, rec2) {
    const [x1, y1, x2, y2] = rec1;
    const [x3, y3, x4, y4] = rec2;
    
    return (!(x2 <= x3 || x4 <= x1)) && (!(y2 <= y3 || y4 <= y1));
};
```

参考题解：
1. https://leetcode-cn.com/problems/rectangle-overlap/solution/836-ju-xing-zhong-die-by-chen-wei-f-akdd/
2. https://leetcode-cn.com/problems/rectangle-overlap/solution/tu-jie-jiang-ju-xing-zhong-die-wen-ti-zhuan-hua-we/

