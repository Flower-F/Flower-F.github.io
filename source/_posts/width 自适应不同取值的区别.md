---
title: 'width 自适应不同取值的区别'
date: 2021-12-30 17:40:03
tags: [css]
copyright: true
---

# width: 100% 与 width: auto 的区别

1. width:100% 并不包含 margin-left 和 margin-right 的属性值，直接取其父容器的宽度加上含 margin-left 和 margin-right 的值。如果设置了 margin 那新的 width 值是容器的宽度加上 margin 的值。

2. width:auto 包含 margin-left 和 margin-right 的属性值。width:auto 总是占据整行，margin 的值已经包含其中了。

3. 一般 width:auto 使用的多，因为这样灵活，而 width:100% 使用比较少，因为在增加 padding 或者 margin 的时候，容易使其突破父级框，破环布局。

举例来说，
当 child 没有加 margin，两者之间没什么区别
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

或者改成
```css
.child {
  width: auto;
}
```

结果都一样
![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/202858.jpg)

但是当 child 加上 margin 后，
```css
.child {
  margin-left: 50px;
}
```

`width: 100%` 的情况会变为
![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/203137.jpg)
`width: auto` 的情况会变为
![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/203252.jpg)