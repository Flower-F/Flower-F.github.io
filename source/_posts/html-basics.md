---
title: html 简单面试题
date: 2021-12-31 14:18:46
tags: [html]
copyright: true
---
# 行内元素、块级元素、空元素有哪些
行内：span, a, label 不独占一行，不能设置宽高
块级：div, footer, header, section, p, h1-h6 独占一行，可以设置宽高
空元素：br, hr 不独占一行，可以设置宽高

# 标签语义化的理解
- HTML 语义化就是让页面的内容结构化，便于对浏览器解析，便于搜索引擎捕获，可以优化 SEO
- 提高代码可读性

# HTML5 的新特性
- 新增选择器 document.querySelector、document.querySelectorAll
- 拖拽释放（Drag and drop） API
- 媒体播放的 video 和 audio
- 本地存储 localStorage 和 sessionStorage
- 离线应用 manifest
- 语义化标签 article、footer、header、nav、section
- 跨域资源共享（CORS） Access-Control-Allow-Origin
- canvas

# 浏览器是怎么对 HTML5 的离线储存资源进行管理和加载的
- 在线的情况下，浏览器发现 html 头部有 manifest 属性，它会请求 manifest 文件，如果是第一次访问 app，那么浏览器就会根据 manifest 文件的内容下载相应的资源并且进行离线存储。如果已经访问过 app 并且资源已经离线存储了，那么浏览器就会使用离线的资源加载页面，然后浏览器会对比新的 manifest 文件与旧的 manifest 文件，如果文件没有发生改变，就不做任何操作，如果文件改变了，那么就会重新下载文件中的资源并进行离线存储
- 离线的情况下，浏览器就直接使用离线存储的资源

# cookie，sessionStorage 和 localStorage 的区别
- cookie 数据始终在同源的 http 请求中携带（即使不需要），也就是说它会在浏览器和服务器之间来回传递；sessionStorage 和 localStorage 不会自动把数据发给服务器，仅在本地保存
- cookie 数据大小不能超过 4k；sessionStorage 和 localStorage 虽然也有存储大小的限制，但比 cookie 大得多，可以达到 5M 甚至更大
- localStorage 存储持久数据，浏览器关闭后数据不丢失除非人为主动删除数据；sessionStorage 数据在当前浏览器窗口关闭后自动删除；cookie 设置的 cookie 过期时间之前一直有效，即使窗口或浏览器关闭

# iframe 的缺点
- iframe 会阻塞主页面的 onload 事件
- 搜索引擎的检索程序无法解读这种页面，不利于 SEO

# src 与 href的区别
src 用于**替换**当前元素，href 用于在当前文档和引用资源之间**确立联系**

- src 是 source 的缩写，指向外部资源的位置，指向的内容将会**嵌入到文档中当前标签所在位置**；在请求 src 资源时会将其指向的资源下载并应用到文档内，例如 js 脚本，img 图片和 iframe 等元素
- href 是 Hypertext Reference 的缩写，指向网络资源所在位置。如果我们在文档中添加 `<link href="common.css" rel="stylesheet"/>` 那么浏览器会识别该文档为 CSS 文件，然后**并行**下载资源并且不会停止对当前文档的处理。这也是为什么建议使用 `link` 方式来加载 CSS，而不是 `@import` 方式