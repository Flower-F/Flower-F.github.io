---
title: 从浏览器地址栏输入 URL 到显示页面的步骤
date: 2021-12-31 14:49:14
tags: [浏览器, 计算机网络, 连载]
copyright: true
---
# Chrome 架构
1. 浏览器进程：界面显示、用户交互、子进程管理、存储
2. 渲染进程：将 HTML、CSS 和 JavaScript 文件转换为网页。默认情况下，Chrome 会为每个 Tab 标签创建一个渲染进程
   - GUI 渲染线程：解析 HTML，CSS，构建 DOM 树和 CSSOM 树，渲染浏览器界面；重绘和回流；注意 GUI 渲染线程与 JS 引擎线程是互斥的，当 JS 引擎线程执行时 GUI 渲染线程会停止运行
   - JS 引擎线程：负责解析 Javascript 脚本，如 V8 引擎；GUI 渲染线程与 JS 引擎线程是互斥的，所以如果 JS 执行的时间过长，这样就会造成页面的渲染不连贯，导致页面渲染加载阻塞
   - 事件处理线程：事件循环；任务队列
   - 定时器线程：通过单独线程来计时并触发定时
   - 异步 http 请求线程：进行异步请求
3. GPU 进程：进行 GPU 加速
4. 网络进程：负责页面的网络资源加载
5. 插件进程：主要是负责插件的运行。因为插件易崩溃，所以需要通过插件进程来隔离，以保证插件进程崩溃不会对浏览器和页面造成影响

# 从浏览器地址栏输入 URL 到显示页面的步骤
## 浏览器线程
- 在浏览器地址栏输入 url
- 浏览器进程通过进程间通信（IPC）把 url 请求发送给网络进程

## 网络进程
- 浏览器查看是否缓存了该请求资源，如果请求资源在缓存中且未过期
  - 如果资源未缓存，发起新请求
  - 如果已缓存，检验缓存是否过期，未过期就直接提供给客户端，否则重新请求（此处涉及缓存相关知识）
- DNS 解析：DNS 解析实际上就是进行网址和 IP 地址的转换。进行 DNS 解析时先查找缓存，没有再使用 DNS 服务器解析，查找顺序为：
  - 浏览器缓存
  - 本机缓存
  - 路由器缓存
  - 若缓存没有，进行递归查询：先在本地的域名服务器中查找，没有就去 com 顶级域名服务器查找，还没找到就去 13 台根域名服务器查找。如此的类推下去，直到找到 IP 地址，然后把它记录在本地，供下次使用
- 等待 TCP 队列：一般而言，对于 http1.1，浏览器会为每个域名最多维护 6 个 TCP 连接，如果发起一个 HTTP 请求时，这 6 个 TCP 连接都处于忙碌状态，那么这个请求就会处于排队状态
- 通过三次握手建立 TCP 连接：TCP 是⾯向连接的协议，所以使⽤ TCP 前必须先建⽴连接，⽽建⽴连接就是通过三次握⼿来进⾏的
  - ⼀开始，客户端和服务端都处于 CLOSED 状态。先是服务端主动监听某个端⼝，处于 LISTEN 状态
  - 客户端会随机初始化一个序号（client_isn），同时把 SYN 标志位置为 1。接着把第⼀个 SYN 报⽂发送给服务端，表示向服务端发起连接，之后客户端处于 SYN-SENT 状态
  ![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/160947.jpg)
  - 服务端收到客户端的 SYN 报⽂后，⾸先服务端也随机初始化⾃⼰的序号（server_isn），其次把 TCP ⾸部的**确认应答号**字段填⼊ client_isn + 1，接着把 SYN 和 ACK 标志位置为 1。最后把该报⽂发给客户端，之后服务端处于 SYN-RCVD 状态
  ![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/161037.jpg)  
  - 客户端收到服务端报⽂后，还要向服务端回应**最后⼀个**应答报⽂，⾸先把 ACK 标志位置为 1 ，其次**确认应答号**字段填⼊ server_isn + 1 ，最后把报⽂发送给服务端，这次报⽂可以携带客户端到服务器的数据（**之前的两次都可以不携带**），之后客户端处于 ESTABLISHED 状态
  ![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/161341.jpg)
  - 服务器收到客户端的应答报⽂后，也进⼊ ESTABLISHED 状态
  - 双⽅都处于 ESTABLISHED 状态，此时连接就已建⽴完成，客户端和服务端就可以相互发送数据了
- 发送 HTTP 请求
- 服务器端处理请求
- 客户端根据响应报文的状态码处理响应
  - 200：检查 Content-Type 字段，如果值为 `text/html` 说明是HTML文档，如果是 `application/octet-stream` 说明是文件下载
  - 301/302：表明服务器已更换域名，需要重定向。这时网络进程会从响应头的 Location 字段里面读取重定向的地址，然后再发起新的 HTTP 或者 HTTPS 请求，跳回到 DNS 解析那一步继续进行
  - 304：表示自从上次请求后，请求的网页未修改过。服务器返回此响应时，不会返回网页内容
- 请求结束，当通用首部字段 Connection 不是 Keep-Alive 时，即不为 TCP 持续连接时，通过四次挥手断开 TCP 连接

## 渲染进程
![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/163032.jpg)
- 解析 HTML 形成 DOM 树
- 解析 CSS 形成 CSSOM 树
- 根据 DOM 树和 CSSOM 树构建渲染树
  - 从 DOM 树的根节点遍历所有可见节点，不可见节点包括：script、meta 这样本身不可见的标签；被 CSS 隐藏的节点，如 `display: none`
  - 根据 CSSOM 规则计算样式
- JS 解析
- 浏览器开始渲染并绘制页面

# 缓存方案

# DNS 解析如何优化

# 为什么是三次握手而不是两次

# POST 和 GET 请求的区别

# 状态码

# 为什么要四次挥手

（未完待续）

参考资料：
https://zhuanlan.zhihu.com/p/102149546
https://mp.weixin.qq.com/s/7kdzH_1E0g332GTrtOyU8w
https://www.cnblogs.com/xuanbingbingo/p/8675791.html
https://www.cnblogs.com/AhuntSun-blog/p/12529920.html
https://juejin.cn/post/6844903832435032072
https://juejin.cn/post/6844903553795014663
公众号小林coding
公众号前端点线面