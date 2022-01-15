---
title: 字节前端青训营第 1 天
date: 2022-01-15 10:12:24
tags: [html, css]
copyright: true
---
# DOCTYPE 的作用
- `<!DOCTYPE>` 声明位于文档中的最前面，处于 `<html>` 标签之前。告知浏览器的解析器用什么文档标准来解析这个文档。如果 `DOCTYPE` 不存在或格式不正确的话，会导致文档以兼容模式呈现。
- 在标准模式（严格模式）下，浏览器的解析规则都是按照最新的标准进行解析的。而在兼容模式（怪异模式）下，浏览器会以向后兼容的方式来模拟老式浏览器的行为，以保证一些老的网站的正确访问。

# 标签语义化的意义
- 便于浏览器的解析与渲染
- 帮助阅读代码的人更容易理解代码的结构，利于代码的维护
- 有利于 SEO
- 实现网页的无障碍阅读

# strong 和 em 的区别
- `em` 用来局部强调，`strong` 则是全局强调
- `em` 的强调是有顺序的，只有阅读到某处时用户才会注意到。`strong` 的强调则是一种随意无顺序的，看见的时候立刻就凸显出来的关键词句。斜体和粗体刚好满足了这两种视觉效果，因此也就成了 `em` 和 `strong` 的默认样式。

# href 和 src 的区别
- src 是 source 的缩写，指向外部资源的位置，指向的内容将会**嵌入到文档中**当前标签所在位置；在请求 src 资源时会将其指向的资源下载并应用到文档内，例如 js 脚本，img 图片和 iframe 等元素
- href 是 Hypertext Reference 的缩写，指向网络资源所在位置。例如说我们在文档中添加 `<link href="common.css" rel="stylesheet"/>`，那么浏览器会识别该文档为 CSS 文件，然后并行下载资源并且不会停止对当前文档的处理

# iframe 的应用和缺点
[iframe 的应用](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/iframe)
缺点：
- iframe 会阻塞主页面的 onload 事件
- 搜索引擎的检索程序无法解读这种页面，不利于 SEO

# 关于 line-height 继承的计算
`line-height` 有三种赋值方式，分别是单位、百分比、纯数字。
- 带单位：px 是固定值，而 em 会参考父元素 font-size 值计算自身的行高
- 纯数字：会把比例传递给后代。例如，父级行高为 1.5，子元素字体为 18px，则子元素行高为 1.5 * 18 = 27px
- 百分比：会将计算后的值传递给后代。例如，父级行高为 150%，font-size 是 20px，那么行高就是 20px * 150% = 30px，子元素也是 30px

# line-height 与 height 的区别
- `line-height` 是每一行文字的高度，如果文字换行，整个盒子的实际高度会变大，高度 = 行数 * 行高
- `height` 本身是一个已经写死的值，就代表盒子高度

# link 和 @import 的区别
- `link` 是 `html` 方式， `@import` 是 `css` 方式。`@import` 只有导入样式表的作用；`link` 不仅可以加载 CSS 文件，还可以定义其他属性，如 `shortcut`
- 加载页面时，`link` 标签引入的 CSS 被同时并行加载；`@import` 引入的 CSS 将在页面加载完毕后被加载，可能会出现 FOUC
- 浏览器对 `link` 支持早于 `@import`，兼容性更好

FOUC（Flash Of Unstyled Content）：用户定义样式表加载之前浏览器使用默认样式显示文档，用户样式加载渲染之后再重新显示文档，造成页面闪烁。

# CSS 什么属性可以继承，什么不能继承？
可以继承：
- 字体相关的属性，`font-size`、`font-weight` 等
- 文本相关的属性，`color`、`line-height` 、`text-align` 等
- 光标属性 `cursor`
- 元素可见性 `visibility`

不可继承：几何属性，如 `padding`、`margin`、`border`

当一个属性不是继承属性的时候，我们也可以通过将它的值设置为 `inherit` 来使它从父元素那获取同名的属性值来继承

# padding 和 margin 赋值为百分比的时候是相对于什么计算的？
在默认的文档流方向下，`margin` 和 `padding` 的水平和垂直方向的百分比值**都是相对于宽度**计算的。

# CSS 画三角形
```html
<div class="triangle">
</div>

<style>
    .triangle {
        width: 0;
        height: 0;
        border-style: solid;
        border-width: 100px;
        border-color: black transparent transparent transparent;
    }
</style>
```

# margin 合并
[蛋老师](https://www.bilibili.com/video/BV1DE41197Kc)

# 盒子模型的类型
有 3 种盒子模型：IE 盒模型（border-box）、标准盒模型（content-box）、padding-box
盒模型一般都由四部分组成：内容（content）、填充（padding）、边界（margin）、边框（border）

盒子模型之间的区别：
- 标准盒模型：width，height 只包含内容 content，不包含 border 和 padding
- IE 盒模型：width，height 包含 content、border 和 padding
- padding-box：width，height 包含 content、padding，不包含 border

# BFC
[BFC 解析](https://github.com/zuopf769/notebook/blob/master/fe/BFC%E5%8E%9F%E7%90%86%E5%89%96%E6%9E%90/README.md)

# CSS 选择器
- id 选择器 #myid
- 类选择器 .classname
- 标签选择器 div
- 后代选择器 div p
- 子选择器 div > p
- 兄弟选择器 div ~ p
- 相邻兄弟选择器 div + p
- 属性选择器 a[rel="external"]
- 伪类选择器 a:hover，li:first-child
- 伪元素选择器 a::before，a:after
- 通配符选择器 *

# 伪元素和伪类的区别
- 伪类用于当已有的元素处于某个状态时，为其添加对应的样式，这个状态是根据用户行为而动态变化的。比如说，当用户悬停在指定的元素时，我们可以通过 `:hover` 来描述这个元素的状态
- 伪元素用于创建一些不在文档树中的元素，并为其添加样式。它们允许我们为元素的某些部分设置样式。比如说，我们可以通过 `::before` 来在一个元素前增加一些文本，并为这些文本添加样式。虽然用户可以看到这些文本，但是这些文本实际上不在文档树中

# width: 100% 与 width: auto 的区别
- `width:100%` 并不包含 `margin-left` 和 `margin-right` 的属性值，直接取其父容器的宽度加上含 `margin-left` 和 `margin-right` 的值。如果设置了 `margin` 那新的 `width` 值是容器的宽度加上 `margin` 的值
- `width:auto` 包含 `margin-left` 和 `margin-right` 的属性值。`width:auto` 总是占据整行，`margin` 的值已经包含其中了
- 一般 `width:auto` 使用的多，因为这样灵活，而 `width: 100%` 使用比较少，因为在增加 `padding` 或者 `margin` 的时候，容易使其突破父级框，破坏布局

举例来说，
当 child 没有加 `margin`，两者之间没什么区别。

以下面这段代码为例，

```html
<style>
.parent {
    height: 200px;
    width: 200px;
    background-color: rgb(195, 229, 236);
    color: #000;
}

.child {
    height: 100px;
    width: 100%;
    background-color: red;
    color: #fff;
}
</style>
```

我们将其改成
```css
.child {
  width: auto;
}
```

结果相同，都是
![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/202858.jpg)

但是如果给 child 加上 `margin` 后，比如：
```css
.child {
  margin-left: 50px;
}
```

`width: 100%` 的情况会变为：
![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/203137.jpg)

`width: auto` 会变为：
![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/203252.jpg)

# 图片空隙问题 & 基线
[vertical-align 戳这](https://developer.mozilla.org/zh-CN/docs/Web/CSS/vertical-align)

图片空隙问题见下面的代码：
```html
<div>
    <img src="https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/ai.jpg" alt="">
    我是哀酱
</div>
```
![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/20220115213134.png)

因为是对齐基线，所以会有一点点空隙，修改方法：
```css
img {
    vertical-align: bottom;
}
```

# 浏览器页面的渲染过程 & 关键渲染路径
[玩转 CSS 的艺术之美](https://juejin.cn/book/6850413616484040711/section/6850413616555491336)

# 行内元素和块级元素的区别
- 块级元素可以设置宽高，独占一行
- 行内元素不可以设置宽高，不独占一行

# flex 布局
[MDN 文档](https://developer.mozilla.org/zh-CN/docs/Web/CSS/flex)

# grid 布局
[MDN 文档](https://developer.mozilla.org/zh-CN/docs/Web/CSS/grid)

# float 图文环绕
根据老师的说法，`float` 现在的使用场景基本只有这个。
双飞翼 & 圣杯：？？？

```html
<style>
    .container {
        width: 300px;
        overflow: hidden;
    }

    img {
        height: 100px;
        float: left;
        margin-right: 10px;
    }
</style>

<div class="container">
    <img src="https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/ai.jpg" alt="你的哀酱">
    <p>
        Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore
        magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo
        consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla
        pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est
        laborum.
    </p>
</div>
```

![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/20220115212154.png)

# 0.5px 问题
Chrome 中文界面下默认会将小于 12px 的文本强制按照 12px 显示。
可以使用 `transform: scale(xx);` 解决。