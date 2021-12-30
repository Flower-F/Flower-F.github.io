---
title: link 和 @import 的区别.md
date: 2021-12-30 21:47:07
tags: [css]
copyright: true
---
# link 与 @import 的区别
- link是 html 方式， @import 是 css 方式。@import 只有导入样式表的作用；link 不仅可以加载 CSS 文件，还可以定义其他属性，如 shortcut
- 加载页面时，link 标签引入的 CSS 被同时并行加载；@import引入的 CSS 将在页面加载完毕后被加载，会出现 FOUC（文档样式短暂失效）
- 浏览器对 link 支持早于 @import，兼容性更好
- 可以通过 JS 操作 DOM ，插入 link 标签来改变样式；JS 无法使用 @import 的方式插入样式

# 补充：什么是 FOUC
FOUC（Flash Of Unstyled Content）：用户定义样式表加载之前浏览器使用默认样式显示文档，用户样式加载渲染之后再重新显示文档，造成页面闪烁。
解决方法：把样式表放到文档的 `<head>`

参考资料：
https://www.cnblogs.com/my--sunshine/p/6872224.html