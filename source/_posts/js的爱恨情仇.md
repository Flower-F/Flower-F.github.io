---
title: js 的爱恨情仇
date: 2021-12-11 19:21:57
tags: [javascript, 连载]
copyright: true
---
记录一下关于 js 的奇葩事情
# typeof
为什么 typeof null 是 object？
js在底层存储变量的时候，会在变量机器码的低位 1-3 位存储其类型信息，其中 object 的低 1-3 位是 000。typeof 通过机器码判断类型，而由于 null 的所有机器码均为 0，该机器码和对象一样，因此 null 直接被当作对象来看待。这个 bug 的改动可能会牵涉到非常多的其他内容变更，所以一直没有被改，因为它的改动很可能会引发更多的其它 bug。

# 除以 0
js 里面 0 居然是可以当除数的。
```js
1 / +0; // Infinity
1 / -0; // -Infinity
-1 / -0; // Infinity
```

# isNaN
如图，记录下这一刻。
![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/201438.jpg)
2021 年 12 月 15 日，leetcode 提交连续 WA 了无数次。最后才知道原来 js 里面 isNaN 判断空格也是 false。

