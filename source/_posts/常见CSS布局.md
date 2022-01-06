---
title: 常见 CSS 布局
date: 2021-12-30 22:43:46
tags: [css]
copyright: true
---

# 流体布局
要求：左右固定，中间自适应。
原理：一个左浮，一个右浮，中间宽度自适应。
```html
<div class="container">
    <div class="left"></div>
    <div class="right"></div>
    <div class="main"></div>
</div>

<style>
    .left {
        float: left;
        width: 100px;
        height: 200px;
        background: red;
    }

    .right {
        float: right;
        width: 200px;
        height: 200px;
        background: blue;
    }

    .main {
        height: 200px;
        background: green;
    }
</style>
```

# 三列布局
要求：三列布局；中间主体内容前置，且宽度自适应；两边内容定宽。
优点：重要内容放在文档流前面，可以被优先渲染

首先完成页面基础布局。
```html
<div class="container">
    <div class="main"></div>
    <div class="left"></div>
    <div class="right"></div>
</div>

<style>
    body {
        /* 避免出现奇怪 bug */
        min-width: 600px;
    }

    .main,
    .left,
    .right {
        float: left;
    }

    .left {
        width: 200px;
        height: 200px;
        background: green;
    }

    .right {
        width: 100px;
        height: 200px;
        background-color: red;
    }

    .main {
        width: 100%;
        height: 200px;
        background: yellow;
    }
</style>
```

## 圣杯布局
- 先给 container 设置左右 padding，把位置空出来
```css
.container {
    padding-left: 200px;
    padding-right: 100px;
    overflow: hidden;
}
```
- left 设置 margin-left 为 -100%，来到最左边
```css
.left {
    margin-left: -100%;
}
```
- right 设置 margin-left 为 -自身宽度，来到最右边
```css
.right {
    margin-left: -100px;
}
```
完成后如图：
![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/232316.jpg)

接下来只要给 left 和 right 分别一个 `position: relative` 将它们自身定位到两端即可。
```css
.left {
    position: relative;
    left: -200px;
}
.right {
    position: relative;
    left: 100px;
}
```

## 双飞翼布局
- 首先在 main 里面添加一个子节点 content，若不设置此节点，main 的内容将无法显示
```html
<div class="container">
    <div class="main">
        <div class="content"></div>
    </div>
    <div class="left"></div>
    <div class="right"></div>
</div>
```
- container 不需要设置 padding
- left 和 right 不需要设置 relative 定位
- 调整子节点 content 位置
```css
.content {
    margin: 0 100px 0 200px;
}
```

# 多列等高布局
考虑兼容性的情况下，可以使用很大的负 margin-bottom 和很大的正 padding-bottom 对冲实现。
```html
<style>
    ul {
        overflow: hidden;
    }

    li {
        float: left;
        margin: 0 10px -9999px 0;
        padding-bottom: 9999px;
        background: #5182e6;
        width: 200px;
        color: #fff;
        list-style: none;
    }

    p {
        padding: 10px;
    }
</style>

<ul>
    <li>
        <p>
            测试测试测试测试测试测试测试测试测试测试测试测试测试测试测试测试测试测试
            测试测试测试测试测试测试测试测试测试测试测试测试测试测试测试测试测试测试
        </p>
    </li>
    <li>
        <p>
            测试测试测试测试测试测试测试测试测试测试测试测试测试测试测试测试测试测试
            测试测试测试测试测试测试测试测试测试测试测试测试测试测试测试测试测试测试
            测试测试测试测试测试测试测试测试测试测试测试测试测试测试测试测试测试测试
            测试测试测试测试测试测试测试测试测试测试测试测试测试测试测试测试测试测试
            测试测试测试测试测试测试测试测试测试
        </p>
    </li>
    <li>
        <p>一家将客户利益置于首位的经纪商</p>
    </li>
</ul>
```
![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/164755.jpg)

参考资料：
https://juejin.cn/post/6955482100426342430