---
title: display、overflow 和 opacity
date: 2021-12-30 21:39:19
tags: [css]
copyright: true
---
# display: none 与 visibility: hidden 与 opacity: 0 的区别
联系：它们都能让元素不可见
区别：
- `opacity: 0` 设置后元素还可以进行交互，如点击事件之类的
- `display:none;` 会让元素完全从渲染树中消失，渲染的时候不占据任何空间；`visibility: hidden;` 不会让元素从渲染树消失，渲染时元素继续占据空间，只是内容不可见
- `display: none;` 是非继承属性，子孙节点消失由于元素从渲染树消失造成，通过修改子孙节点属性无法显示；`visibility: hidden;` 是继承属性，子孙节点消失由于继承了 hidden，通过设置 `visibility: visible;` 可以让子孙节点显式
- 修改 display 通常会造成文档重排；修改 visibility 只会造成元素重绘
- 读屏器不会读取 `display: none;` 元素内容；会读取 `visibility: hidden;` 元素内容