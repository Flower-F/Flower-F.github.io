---
title: 前端冷知识
date: 2022-01-07 23:24:23
tags: [javascript, 连载]
copyright: true
---
本文摘自[字节青训营社区技术问答板块](https://forum.juejin.cn/youthcamp/category/6950604566098346019)

# #1 在什么情况下 a === a - 1 ？（上）
- 正负 Infinity
![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/20220107232953.png)
- 给 a 赋一个很大的值（准确地说，是超过 `Number.MAX_SAFE_INTEGER` 的数）
![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/20220107233143.png)
这是因为在 JavaScript 里，整数可以被精确表示的范围是从 `-2 ** 53 + 1` 到 `2 ** 53 - 1`，即 -9007199254740991 到 9007199254740991。超过这个数值的整数，都不能被精确表示。
- 注意 NaN 跟任何值都不相等，包括 NaN 本身
![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/20220107233346.png)

# #2 在什么情况下 a === a - 1 ？（下）
使用 `defineProperty` 函数
```js
let count = 0;
Object.defineProperty(window, 'a', {
  get() {
    return ++count;
  }
});
console.log(a === a - 1); // true
```

