---
title: 元素居中的方法
date: 2021-12-30 16:10:52
tags: [css]
copyright: true
---
# 水平居中
## margin: 0 auto
```css
子元素块级元素，设置 `margin: 0 auto`
```

## text-align: center
子元素行内元素，父亲设置 `text-align: center` 即可

# 水平垂直居中
## 负数 margin
```css
div {
    position: absolute;
    width: 100px;
    height: 150px;
    top: 50%;
    left: 50%;
    margin-left: -50px;
    margin-top: -75px;
}
```

## margin: auto
```css
div {
    position: absolute;
    width: 100px;
    height: 100px;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    margin: auto;
}
```

## transfrom
```css
div {
    position: absolute;
    width: 100px;
    height: 100px;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
}
```

## flex
```css
div {
    display: flex;
    justify-content: center;
    align-items: center;
}
```

参考资料：
https://www.jianshu.com/p/a7552ce07c88