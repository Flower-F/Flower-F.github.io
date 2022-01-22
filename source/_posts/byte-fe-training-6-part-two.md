---
title: 字节前端青训营第 6 天（下）
date: 2022-01-21 14:05:55
tags: [web安全]
copyright: true
---
# XSS

Cross-Site Scripting，跨站脚本攻击。指攻击者通过某种方式把恶意脚本注入你写的页面。

![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/20220121214536.png)

## 产生原因

- 开发者盲目相信用户提交的内容
- 直接把用户的提交转换成了 DOM，如 `document.write(xxx)`、`elem.innerHTML = xxx`

## 特点

- 通常难以从 UI 上感知
- 窃取用户信息（cookie / token）
- 因为可以操作 js，所以可以绘制 UI，诱骗用户填写表单

## 示例

```js
async function submit(ctx) {
  const { content, id } = ctx.request.body;
  // 没有对 content 进行过滤
  await db.save({
    content,
    id
  });
}

async function render(ctx) {
  const { content } = await db.query({
    id: ctx.query.id
  });
  // 又没有对 content 进行过滤
  ctx.body = `<div>${content}</div>`;
}
```

导致攻击：

![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/20220121215645.png)

## Stored XSS 存储型

- 恶意脚本被保存在数据库中
- 访问页面 -> 读数据 -> 被攻击
- 危害最大，对全部用户可见

比如说一个用户在视频中插入一个 XSS 攻击，然后某一刻脚本被启动了，此时所有的在浏览这个页面的用户都会被脚本攻击，造成信息泄露。

## Reflected XSS 反射型

不涉及数据库，而是从 URL 进行攻击

![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/20220121220034.png)

把字段直接生成 HTML 字段，然后被成功攻击

![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/20220121220057.png)

## Dom-based XSS

不需要服务器参与，恶意攻击的发起与执行都在浏览器完成。

![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/20220121220034.png)

![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/20220121220433.png)

## Mutation XSS

利用了浏览器渲染 DOM 的特性，对于不同的浏览器执行会有区别。

![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/20220121220622.png)

代码会被渲染为：

![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/20220121220828.png)

又由于 src 属性不符合规范，然后会触发 `onerror` 事件，也就完成了 XSS 攻击。

# CSRF 

Cross-site request forgery，跨站请求伪造

## 特点

- 用户不知情
- 利用用户权限
- 构造指定的 HTTP 请求，窃取或修改用户的敏感信息

## 示例

![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/20220121221305.png)

![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/20220121221625.png)

在这个例子中，用户并没有直接请求银行，但是这个请求却被成功执行了，这就是一个经典的 CSRF 攻击。

# Injection 注入

## SQL Injection

![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/20220121221710.png)

删库跑路示例：

![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/20220121223501.png)

其余注入：
- CLI

![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/20220121224200.png)

![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/20220121224547.png)

因为没有过滤导致成功删除跑路

- 读取 + 修改

以 Nginx 为例，如果用户可以读取 Nginx 的配置文件，就能把我们的网站转到另一个网站

- SSRF Server-Side Request Forgery（严格来说不算注入）

![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/20220121232106.png)

# DOS

Denial of Service，服务拒绝。通过某种方式构造特定的请求，导致服务器资源被显著消耗，来不及响应更多的请求，导致请求积压，进而引发雪崩效应。

## Regex DOS

## 正则贪婪模式

**书写正则的时候是否写 ?**

![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/20220121232454.png)

这里的第一行就是贪婪模式

![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/20220121232755.png)

因为贪婪引发回溯

## DDOS 分布式拒绝服务

短时间内，收到大量来自僵尸设备的请求流量，服务器不能及时完成全部的请求，导致请求堆积，进而引发雪崩效应，无法响应新的请求。

*不搞复杂的，量大就完事儿*

### 攻击特点
- 直接访问 IP 而不是域名
- 使用任意的 API
- 消耗掉大量的带宽，直至耗尽

### 洪水攻击

![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/20220121233159.png)

攻击者发起大量的 TCP 请求，然后就会产生大量的 SYN，发送给服务器。然后服务器就会产生大量的 ACK 和 SYN 给攻击者。但是，攻击者不会返回第三次 ACK，进而导致三次握手失败，连接无法被释放，于是很快就会到达最大连接次数，所有的新请求就无法被响应。

# 中间人攻击

![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/20220121233703.png)

为什么中间人攻击可以成立？

- HTTP 报文使用明文方式发送，可能被第三方窃听。
- HTTP 报文可能被第三方拦截后修改通信内容，接收方没有办法发现报文内容的修改。
- HTTP 还存在认证的问题，第三方可以冒充他人参与通信。

# XSS 防御

- 永远不要信任用户提交的任何内容
- 永远不要把用户提交的内容直接转换成 DOM，而应该转换成字符串
- 主流的框架（React & Vue）其实默认会防御 XSS 攻击

**如果有需求不讲武德，必须动态生成 DOM 呢？**

- 如果要把 string 直接生成 DOM，必须要对 string 进行转义
- 如果允许上传 SVG 文件，需要对 SVG 文件进行扫描，因为 SVG 中允许嵌套 script 标签
- 如果允许用户自定义跳转链接，必须进行检查过滤
![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/20220121234430.png)
像上图这样，用户可以插入 js 代码
- 如果允许自定义样式，必须进行检查过滤
![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/20220121234605.png)

# CSP

Content Security Policy

- 允许开发者定义哪些源（域名）是安全的
- 来自安全源的脚本可以执行，否则直接报错
- 对于 eval 或内联的脚本直接报错

![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/20220121235018.png)

第一行：只允许同源；第二行：除了同源之外，还另外允了 domain.com

我们也可以在浏览器端进行设置：

![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/20220121235151.png)

# CSRF 防御

只要我们限制请求的来源，就可以限制伪造请求。可以根据 Origin 或者 Referer 判断。

## token 防御机制

![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/20220121235423.png)

- token 必须和具体的用户绑定，才能确保不会被其它的用户所利用。
- token 必须有过期时间，否则万一 token 泄露，之前的所有请求都可以被利用

## iframe 攻击

![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/20220121235650.png)

首先在视觉上，让 button 覆盖住 iframe，然后用户就看不出来了。接着通过 button 的设置导致点击事件穿透了，然后传递给了 iframe，iframe 中的请求没有跨域，因此可以完成攻击。

可以设置 HTTP 响应头 `X-Frame-Options: DENY/SAMEORIGIN` （不允许加载 iframe 或只允许加载同源的 iframe）

## CSRF 反模式

![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/20220122000110.png)

GET !== GET + POST

一旦被攻击，信息不单止可能泄露，甚至还会被篡改。

## SameSite Cookie

限制 cookie 的 domain 属性。我页面的 cookie 只能为我所用，只有同域才能使用这个 cookie，第三方服务的请求不能带上我页面的 cookie。

**但是如果服务依赖于第三方的 cookie 怎么办？**

比如内嵌了一个 b 站的播放器，需要识别用户的登录状态。

可以设置 `SetCookie: SameSite=None; Secure;`

即不限制 same site，但是必须确保 cookie 是安全的（只能通过 HTTPS 传输）。

**SameSite VS 同源策略**：SameSite 主要针对 cookie，同源策略 针对的是请求的资源。

## 防御 CSRF 的正确姿势

用 Node 做一个中间件防范攻击。

# Injection 防御

## SQL 注入

对 SQL 语句做一些 prepare 处理

![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/20220122095147.png)

## 其余注入

- 命令不要通过 sudo 执行，不要给 root 权限
- 拒绝像 rm 这种极其危险的行为
- 对 URL 类型参数进行协议、域名、IP 等的限制

# DOS 防御

## Regex DOS

- 避免写出贪婪的正则匹配
- 扫描代码找出里面的所有正则，然后做正则性能测试
- 拒绝使用用户提供的正则

## DDOS

过滤：
- 负载均衡
- API 网关

抗量：
- 快速自动扩容
- 非核心服务降级

# 传输层防御

使用 HTTPS，其中 HTTP3 内置了 TLS

![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/31121131121.png)

# HSTS

HTTP Strict Transport Security，HTTO 严格传输安全协议。

HSTS 的作用是强制客户端（如浏览器）使用 HTTPS 与服务器创建连接。服务器开启 HSTS 的方法是，当客户端通过 HTTPS 发出请求时，在服务器返回的超文本传输协议（HTTP）响应头中包含 Strict-Transport-Security 字段。非加密传输时设置的 HSTS 字段无效。
比如，https://example.com/ 的响应头含有 Strict-Transport-Security: max-age=31536000; includeSubDomains。这意味着两点：
- 在接下来的 31536000 秒（即一年）中，浏览器向 example.com 或其子域名发送 HTTP 请求时，必须采用 HTTPS 来发起连接。比如，用户点击超链接或在地址栏输入 http://www.example.com/ ，浏览器应当自动将 http 转写成 https，然后直接向 https://www.example.com/ 发送请求。
- 在接下来的一年中，如果 example.com 服务器发送的 TLS 证书无效，用户不能忽略浏览器警告继续访问网站。

# SRI

Subresource Integrity，子资源完整性。

Web 性能优化中很重要的一点是加快请求完成速度，让可缓存的资源走 CDN 是最通用的做法。CDN 服务提供商通过分布在各地的节点，让用户从最近的节点加载内容，大幅提升速度。但是 CDN 的安全性一直是一个风险点：对于网站来说，让请求从第三方服务器经过，由第三方响应，安全方面肯定不如自己服务器可控。

我们知道 CSP（Content Security Policy） 的外链白名单机制可以在现代浏览器下减小 XSS 风险。但针对 CDN 内容被篡改而导致的 XSS，CSP 并不能防范，因为网站所使用的 CDN 域名，肯定在 CSP 白名单之中。这时候，SRI 就应运而生了。

它通过对比 hash 值，来确保文件的安全性，可以在一定程度上防范 XSS 攻击。

![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/20220122101625.png)


