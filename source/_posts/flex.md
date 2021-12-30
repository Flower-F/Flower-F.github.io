---
title: Flex
date: 2021-12-30 16:26:00
tags: [css]
copyright: true
---
# Flex
flex 布局 是 CSS3 新增的布局方式，我们可以通过将一个元素的 display 属性值设置为 flex 从而使它成为一个 flex 容器，它的所有子元素都会成为它的项目。

一个容器默认有两条轴，一个是主轴，一个是与主轴垂直的交叉轴。我们可以使用 flex-direction 来指定主轴的方向，使用 justify-content 来指定元素在主轴上的排列方式，使用 align-items 来指定元素在交叉轴上的排列方式，还可以使用 flex-wrap 来规定当一行排列不下时的换行方式。

对于容器中的项目，我们可以使用 order 属性来指定项目的排列顺序，使用 flex-grow 来指定当排列空间有剩余的时候，项目的放大比例，使用 flex-shrink 来指定当排列空间不足时，项目的缩小比例。
