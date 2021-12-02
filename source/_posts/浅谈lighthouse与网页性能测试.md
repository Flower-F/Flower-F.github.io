---
title: 浅谈 lighthouse 与网页性能测试
date: 2021-12-02 13:03:45
tags: [测试, 性能]
copyright: true
author: Flower-F
---
今天在写人机交互课程的实验报告，复习了一下之前学到的 web 性能分析的方法，然后突发奇想也给我的网页做了个性能测试，当然结果很不乐观（

# 网页性能分析的重要性

简单来说，一方面，提高用户体验；另一方面，提高网页的 SEO 排名。



# 整体分析

![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/lighthouse.jpg)

先来看整体的分析情况，Performance 是 61 分，SEO 是 97 分。

性能只能说很不理想，但是可惜的是我没有在新建博客的第一时间就先测一下，以至于我不太清楚影响性能的是 hexo + next 本身的问题，还是因为我后来做的美化导致的。另一方面 hexo 是模板建站，我对于里面的源码基本毫不了解，想要优化也不知道从何做起。总之先来看看这 6 个指标吧。

# lighthouse 分析网页的六大指标

![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/115537.jpg)

简单介绍一下这 6 个指标：

## FCP

FCP（First Content Paint），即首次内容绘制，是指浏览器从响应用户输入网络地址，在页面首次绘制文本，图片（包括背景图）、非白色的 canvas 或者 svg 之间的这段时间。以往的话，一般还会提到像 FP（First Paint）、FMP（First Meaning Paint）这两个概念，不过现在已经被 lighthouse 废弃了，FP 指标可以通过 Performance 获取。

## TTI

TTI（Time to Interactive），即可交互时间，指的是网页第一次达到可以交互状态的时间，可交互状态的 UI 组件可以交互，而且页面已经达到了相对稳定流畅的程度。

说到 TTI 一般还会提到 **RAIL** 性能模型，包含四个指标，分别是 Response （响应）、Animation（动画）、Idle（空置状态）和 Load（加载）。

- Response：如果用户点击了一个按钮，你需要保证在用户察觉出延迟之前就得到反馈。只要有输入，这个原则就适用。如果没有在合理的时窗内完成响应，，用户就会察觉到这个延迟，一般而言在这个时间会被定为 100 毫秒。另外，如果用户等待时间可能超出 500ms 的话，需要加上 loading 动画来缓解用户的紧张焦虑。

- Animation：如果动画帧率发生变化，用户很可能会注意到。一般而言，web 性能优化的目标是达到每秒生成 60 帧。谈到 Animation，就会想起浏览器的渲染流程。

  ![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/2011110316263715.png)

  一般来说渲染流程会被分为这四步，明天的主要笔记内容大概会与此相关；今天主要谈 web 性能测试，就先不细说这部分了。

- Idle：可以利用空闲的时间来完成被推迟了的工作。例如，尽可能减少预加载数据，然后利用空闲时间加载剩余的数据。

- Load：最终目标是能够在 1000 毫秒以内呈现页面内容，这方面涉及到关键路径的优化问题，我也放到明天再谈。注意这里 1s 内并不需要渲染出所有的页面内容，可以把还未完成的任务留到空闲时间继续完成。

## SI

SI（Speed Index），即首屏时间，代表页面内容渲染所消耗的时间。优化 Speed Index 一般从两方面入手：优化内容效率和优化关键渲染路径。SI 低于 4s 则表示页面加载速度较优。

## TBT

TBT（Total Blocking Time）是衡量用户事件响应的指标。TBT会统计在FCP和TTI时间之间，主线程被阻塞的时间总和。当主线程被阻塞超过 50ms 导致用户事件无法响应，这样的阻塞时长就会被统计到TBT中。TBT越小说明页面能够更好的快速响应用户事件。

## LCP

LCP（Largest Content Paintful）是一个页面加载时长的技术指标，用于表示当前页面中占比最大的内容显示出来的时间点。它可以代表当前页面主要内容展示的时间。LCP低于 2.5s 则表示页面加载速度较优。

## CLS

CLS（Cumulative Layout Shift）是一个衡量页面内容是否稳定的指标，CLS会将页面加载过程中非预期的页面布局的累积。CLS的分数越低，表明页面的布局稳定性越高，通常低于0.1表示页面稳定性良好。

# lighthouse 的优势

lighthouse 在指出性能不足之处的时候，一般还会具体指出哪些地方可以进行优化，例如这次分析，

![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/125840.jpg)

# 后续

接下来我先考虑的是要把之前鼠标的交互动画减少一些，然后想办法压缩一下静态资源。去除无用 CSS 和 JS 稍微有些难办，因为里面的代码对我来说就像一个黑盒一样，我也没有足够的课余时间去阅读源码。

参考文章：
https://www.cnblogs.com/frank-link/p/15243695.html
https://www.cnblogs.com/loveyt/p/13582359.html
https://blog.csdn.net/m0_37411791/article/details/106394219
https://zhuanlan.zhihu.com/p/20276064