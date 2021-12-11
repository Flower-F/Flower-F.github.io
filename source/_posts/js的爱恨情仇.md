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

