---
title: 浏览器的 CSS 兼容性问题
date: 2021-12-30 16:51:48
tags: [css, 浏览器]
copyright: true
---
# 浏览器默认的 margin 和 padding 不同
解决方案：加一个全局的 `* {margin: 0; padding: 0;}` 来统一

# 双边距
在 IE6 下，如果对元素设置了浮动，同时又设置了 margin-left 或 margin-right，margin 值会加倍
```css
#box {
    float: left;
    width: 10px;
    margin: 0 0 0 10px;
}
```
这种情况之下 IE6 会产生 20px 的距离
解决方案：在float的标签样式控制中加入 `_display:inline;` 将其转化为行内属性(_这个符号只有 IE6 会识别)

# event 对象
IE 下，event 对象有 x、y 属性，但是没有 pageX、pageY 属性；Firefox下，event 对象有 pageX、pageY属性，但是没有 x、y 属性

# Chrome 文本大小
Chrome 中文界面下默认会将小于 12px 的文本强制按照 12px 显示
可以使用 `-webkit-transform: scale(0.5);` 
注意 `-webkit-transform: scale(0.75);` 收缩的是整个 span 的大小，这时候，必须要将 span 转换成块元素，可以使用 `display：block/inline-block/...`。

# hover
超链接访问过后 hover 样式就不出现了，被点击访问过的超链接样式不再具有 hover 和 active 了
解决方法：改变 CSS 属性的排列顺序 L-V-H-A，即 link > visited > hover > active

# 透明度
IE8 以下，没有 opacity 属性
解决方法：
```css
div {
    opacity: 0.5;
    filter: alpha(opacity=50);
}
```