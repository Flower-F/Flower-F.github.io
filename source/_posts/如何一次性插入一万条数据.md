---
title: 如何一次性插入一万条数据
date: 2021-12-30 22:32:05
tags: [css, javascript]
copyright: true
---
如果要在一个 ul 节点中一次性插入 10000 个 li 元素怎么办？

```html
<ul id="parent">
    <!-- 插入一万条数据 -->
</ul>
```

# 常规解法
```js
const parent = document.getElementById("parent");
for (let i = 0; i < 10000; i++) {
    const child = document.createElement("li");
    const text = document.createTextNode(i.toString());
    child.appendChild(text);
    parent.appendChild(child);
}
```
这样对 DOM 反复操作会导致页面重绘、回流，效率非常低，而且页面可能会被卡死。

# DocumentFragment
DocumentFragment 翻译过来就是文档片段的意思。
简单来说就是在内存中提供了一个 DOM 对象，当我们需要频繁操作 DOM 的时候，可以在内存先中创建一个 DocumentFragment 文档片段，然后对这个文档片段中进行一系列频繁的 DOM 操作，当操作结束后直接插入真实的 DOM 节点中，这样所有的节点会被一次插入到真实的文档中，而这个操作仅发生一个**重绘**的操作。

```js
const parent = document.getElementById("parent");
const fragment = document.createDocumentFragment();
for (let i = 0; i < 10000; i++) {
    const child = document.createElement("li");
    const text = document.createTextNode(i.toString());
    child.appendChild(text);
    fragment.appendChild(child);
}
parent.appendChild(fragment);
```

参考资料：
https://juejin.cn/post/6950553806484537357