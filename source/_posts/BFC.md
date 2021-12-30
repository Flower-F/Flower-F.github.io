---
title: BFC
date: 2021-12-30 14:43:02
tags: [css]
copyright: true
---
# BFC
BFC，即 block-formatting-context，块级格式化上下文。通俗来说，BFC 是一个完全独立的区域，在区域内的子元素不会影响到外面的布局。

# BFC 创建方法
- 根元素（html）
- 浮动元素
- 定位为 absolute 或 fixed
- 行内块元素（display: inline-block）
- overflow 的值不为 visible
- flex box（display: flex 或 inline-flex）

# BFC 的特征
- 属于同一个 BFC 的两个相邻元素的 margin 会发生重叠，不同的 BFC 之间 margin 不重合（解决 margin 重叠）
- 计算 BFC 的高度时，浮动元素也参与计算（清除浮动）
- BFC 的区域不会与 float 元素重叠（两栏布局）

# BFC 可以解决的问题
## 防止 margin 重叠
### 使用 BFC 前
```html
<style>
    p {
        color: #f55;
        background: #fcc;
        width: 200px;
        line-height: 100px;
        text-align:center;
        margin: 100px;
    }
</style>

<body>
    <p>A</p>
    <p>B</p>
</body>
```
![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/150941.jpg)
因为 margin 塌陷，实际两者之间只间隔了 100 px。

### 使用 BFC 后
```html
<style>
    .wrap {
        overflow: hidden;
    }
</style>

<body>
    <p>A</p>
    <div class="wrap">
        <p>B</p>
    </div>
</body>
```
![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/151538.jpg)

水平方向和嵌套同理。

## 清除浮动
### 使用 BFC 前
```html
<style>
    .parent {
        border: 5px solid #fcc;
        width: 300px;
    }

    .child {
        border: 5px solid #f66;
        width: 100px;
        height: 100px;
        float: left;
    }
</style>

<body>
    <div class="parent">
        <div class="child"></div>
        <div class="child"></div>
    </div>
</body>
```
![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/151744.jpg)

### 使用 BFC 后
```css
.parent {
    overflow: hidden;
}
```
![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/152029.jpg)

## 自适应两栏布局
### 使用 BFC 前
```html
<style>
    body {
        width: 300px;
        position: relative;
    }

    .aside {
        width: 100px;
        height: 150px;
        float: left;
        background: #f66;
    }

    .main {
        height: 200px;
        background: #fcc;
    }
</style>

<body>
    <div class="aside"></div>
    <div class="main"></div>
</body>
```
![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/152919.jpg)

### 使用 BFC 后
```css
.main {
    overflow: hidden;
}
```
![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/153139.jpg)

参考资料：
https://github.com/zuopf769/notebook/blob/master/fe/BFC%E5%8E%9F%E7%90%86%E5%89%96%E6%9E%90/README.md