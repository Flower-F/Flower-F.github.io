---
title: 跨域解决方案
date: 2022-01-17 20:48:17
tags: [浏览器]
copyright: true
---
# 浏览器同源策略
浏览器同源策略是一个安全策略，其中同源指的是 `协议 + 域名 + 端口号` 三者相同，即使有两个不同的域名指向同一个 IP 地址，也不是同源的。同源策略可以一定程度上防止 XSS、CSRF 攻击。

一个域名的组成包括：
![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/20220117211107.png)

在默认情况下 http 可以省略端口 80， https 可以省略端口 443。也就是说，https://www.baidu.com 和 https://www.baidu.com:443 显然也是同源的，因为它们是一回事。

不符合同源策略导致的后果有：
- localStorage、sessionStorage、Cookie 等浏览器的内存无法跨域访问
- DOM 节点无法进行跨域操作
- Ajax 请求无法跨域请求

但是有一些标签是允许跨域加载资源：
- `<img>`
- `<link>`
- `<script>`

值得注意的几个要点有：
- 如果是协议和端口造成的跨域问题，前端是无能为力的
- **跨域并不是请求发不出去，请求能发出去，服务端能收到请求并正常返回结果，只是结果被浏览器拦截了**

# 代码示例
```js
// 服务端 http://127.0.0.1:8000
const http = require('http');

const port = 8000;

http.createServer((req, res) => {
    res.end(JSON.stringify('hello world'));
}).listen(port, function () {
    console.log('server is listening on port ' + port);
})
```

```html
<!-- 客户端 http://127.0.0.1:5500/index.html -->
<script>
    // 第一步：创建 xhr 对象
    var xhr = new XMLHttpRequest();
    // 第二步：初始化，设置请求方法和 url，注意此处 url 必须写完整
    xhr.open('get', 'http://127.0.0.1:8000');
    // 第三步：发送请求
    xhr.send();
    // 第四步：绑定事件
    xhr.onreadystatechange = function () {
        // readState
        // 0 表示未初始化
        // 1 表示 open 完毕
        // 2 表示 send 完毕 
        // 3 表示服务端返回了部分结果 
        // 4 表示服务端返回了所有结果
        if (xhr.readyState === 4 && xhr.status === 200) {
            // 第五步：处理结果
            console.log(xhr.responseText);
        }
    }
</script>
```

果不其然报错了：
![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/20220117213331.png)

# JSONP
JSONP，即 JSON with Padding，是一个非官方的跨域解决方案，纯粹凭借程序员的聪明才智开发出来，只支持 get 请求。
JSONP 工作原理：在网页有一些标签天生就具有跨域能力，比如 `img` `link` `script` 等。JSONP 就是利用 `script` 标签的跨域能力来发送请求的。

![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/20220117230214.png)
举个例子，客户端传入 a 和 b，服务端传回 a + b 的结果。

```js
// 服务端 http://127.0.0.1:8000
const http = require('http');
const url = require('url');

const port = 8000;

http.createServer((req, res) => {
    const { query } = url.parse(req.url, true);
    const { a, b, callback } = query;
    const ans = parseInt(a) + parseInt(b);
    res.end(`${callback}('${ans}')`);
}).listen(port, () => {
    console.log('server is listening on port ' + port);
})
```

```html
<!-- 客户端 http://127.0.0.1:5500/index.html -->
<body></body>
<script>
    function add(ans) {
        console.log('a + b =', ans);
    }

    const jsonp = (a, b, callback) => {
        const script = document.createElement('script');
        script.src = `http://127.0.0.1:8000?a=${a}&b=${b}&callback=${callback}`;
        document.body.appendChild(script);
    }

    jsonp(1, 2, 'add');
</script>
```

缺点：需要前后端配合，只支持 get 请求。

# CORS
CORS 全称是 Cross-Orgin Resource Sharing，跨域资源共享。CORS 由后端开启，开启后前端就可以跨域访问后端。

服务端设置 `Access-Control-Allow-Origin` 就可以开启 CORS。该属性表示哪些域名可以访问资源，如果设置为通配符 *，则表示所有网站都可以访问资源。类似的还有 `Access-Control-Allow-Methods`，表示允许的请求方法，`Access-Control-Allow-Headers`，表示允许的请求头类型。

设置 CORS 本身和前端没什么关系，但是通过这种方式解决跨域问题的话，会在发送请求时出现两种情况，分别为**简单请求**和**复杂请求**。

## 简单请求
只要同时满足以下条件的就是简单请求：
1. 请求方法是以下三者之一：
- GET
- POST
- HEAD

2. 允许人为设置的字段仅限以下几种：
- Accept
- Accept-Language
- Content-Language
- Content-Type（有额外限制）

3. Content-Type 取值为以下三者之一：
- text/plain
- multipart/form-data
- application/x-www-form-urlencoded

4. 请求中的任意 XMLHttpRequest 对象均没有注册任何事件监听器；XMLHttpRequest 对象可以使用 XMLHttpRequest.upload 属性访问。

5. 请求中没有使用 ReadableStream 对象。

## 复杂请求
不是简单请求的请求就是复杂请求。**复杂请求**必须首先使用 `OPTIONS` 请求方法发起一个**预检请求**到服务器，以获知服务器是否允许该实际请求。预检请求的使用，可以避免跨域请求对服务器的用户数据产生预期之外的影响。

代码示例如下：
```html
<!-- 客户端 http://127.0.0.1:5500/index.html -->
<script>
    // 第一步：创建 xhr 对象
    var xhr = new XMLHttpRequest();
    // 第二步：初始化，设置请求方法和 url，注意此处 url 必须写完整
    xhr.open('get', 'http://127.0.0.1:8000?a=1&b=2');
    // 第三步：发送请求
    xhr.send();
    // 第四步：绑定事件
    xhr.onreadystatechange = function () {
        // readState
        // 0 表示未初始化
        // 1 表示 open 完毕
        // 2 表示 send 完毕 
        // 3 表示服务端返回了部分结果 
        // 4 表示服务端返回了所有结果
        if (xhr.readyState === 4 && xhr.status === 200) {
            // 第五步：处理结果
            console.log(xhr.responseText);
        }
    }
</script>
```

```js
// 服务端 http://127.0.0.1:8000
const http = require('http');
const url = require('url');

const port = 8000;

http.createServer((req, res) => {
    // 开启 CORS
    res.writeHead(200, {
        //设置允许跨域的域名，也可设置 * 表示允许所有域名
        'Access-Control-Allow-Origin': 'http://127.0.0.1:5500',
        //跨域允许的请求方法，也可设置 * 表示允许所有方法
        "Access-Control-Allow-Methods": "DELETE,PUT,POST,GET,OPTIONS",
        //允许的请求头类型
        'Access-Control-Allow-Headers': 'Content-Type'
    })
    const { query: { a, b } } = url.parse(req.url, true);
    res.end(`${a} + ${b} = ${parseInt(a) + parseInt(b)}`);
}).listen(port, function () {
    console.log('server is listening on port ' + port);
})
```

# WebSocket
Websocket 属于应用层协议，它基于 TCP 传输协议，并复用 http 的握手通道。
相比于 http 协议，它的优点是：
- 支持双向通信，客户端和服务器之间存在持久的连接，而且双方都可以随时开始发送数据
- 有更好的二进制支持
- 支持拓展

因为这种方式本质没有使用 http 的响应头, 因此也没有跨域的限制。

**那么为什么 WebSocket 可以跨域呢？**
因为 WebSocket 根本不属于同源策略，而且它本身就有意被设计成可以跨域的一个手段。由于历史原因，跨域检测一直是由浏览器端来做，但是 WebSocket 出现以后，对于 WebSocket 的跨域检测工作就交给了服务端，浏览器仍然会带上一个 Origin 跨域请求头，服务端则根据这个请求头判断此次跨域 WebSocket 请求是否合法。

依然以 a + b 问题举例：

```html
<!-- 客户端 http://127.0.0.1:5500/index.html -->
<script>
    function ws(a, b) {
        const socket = new WebSocket('ws://127.0.0.1:8000');
        socket.onopen = () => {
            socket.send(JSON.stringify({ a, b }));
        };
        socket.onmessage = e => {
            console.log(e.data);
        }
    }

    ws(1, 2);
</script>
```

```js
// 服务端 http://127.0.0.1:8000
const WebSocket = require('ws');

const port = 8000;

const ws = new WebSocket.Server({ port });
ws.on('connection', obj => {
    obj.on('message', data => {
        data = JSON.parse(data.toString());
        const { a, b } = data;
        obj.send(`${a} + ${b} = ${parseInt(a) + parseInt(b)}`);
    })
})
```

# Node 接口代理
同源策略只在浏览器存在，无法限制后端。也就是说前端与后端之间会受同源策略影响，但是后端与后端之间不会受到限制。所以可以通过 Node 做一层接口代理，先访问已经设置了 CORS 的后端 1，再让后端 1 去访问后端 2，获取数据后传给后端 1，最后再让后端 1 把数据传回给前端。

客户端代码同上，把请求端口改成 8888 即可。

```js
// 后端 1 http://127.0.0.1:8888
const http = require('http');
const url = require('url');
const querystring = require('querystring');

const port = 8888;

http.createServer((req, res) => {
    // 开启 CORS
    res.writeHead(200, {
        //设置允许跨域的域名，也可设置 * 表示允许所有域名
        'Access-Control-Allow-Origin': 'http://127.0.0.1:5500',
        //跨域允许的请求方法，也可设置 * 表示允许所有方法
        "Access-Control-Allow-Methods": "DELETE,PUT,POST,GET,OPTIONS",
        //允许的请求头类型
        'Access-Control-Allow-Headers': 'Content-Type'
    })
    const { query } = url.parse(req.url, true);
    const { methods = 'GET', headers } = req;

    // 给后端 2 发送请求
    http.request({
        host: '127.0.0.1',
        port: '8000',
        path: `/?${querystring.stringify(query)}`,
        methods,
        headers
    }, proxyRes => {
        // 把从后端 2 获取的数据传回给前端
        proxyRes.on('data', chunk => {
            console.log(chunk.toString());
            res.end(chunk.toString());
        })
    }).end()
}).listen(port, () => {
    console.log('server is listening on port ' + port);
})
```

```js
// 后端 2 http://127.0.0.1:8000
const http = require('http');
const url = require('url');
const port = 8000;

http.createServer(function (req, res) {
    // 开启 CORS
    res.writeHead(200, {
        //设置允许跨域的域名，也可设置 * 表示允许所有域名
        'Access-Control-Allow-Origin': 'http://127.0.0.1:5500',
        //跨域允许的请求方法，也可设置 * 表示允许所有方法
        "Access-Control-Allow-Methods": "DELETE,PUT,POST,GET,OPTIONS",
        //允许的请求头类型
        'Access-Control-Allow-Headers': 'Content-Type'
    })
    const { query: { a, b } } = url.parse(req.url, true);
    res.end(`${a} + ${b} = ${parseInt(a) + parseInt(b)}`);
}).listen(port, function () {
    console.log('server is listening on port ' + port);
})
```

# Nginx 反向代理
实现原理类似于上面提到的 Node 接口代理，需要你搭建一个中转 Nginx 服务器，用于转发请求。使用 Nginx 反向代理实现跨域，是最简单的跨域方式。只需要修改 Nginx 的配置即可解决跨域问题，支持所有浏览器，支持 session，不需要修改任何代码，并且不会影响服务器的性能。

先根据[Nginx安装教程](https://blog.csdn.net/diaojw090/article/details/89135073)进行 Nginx 的安装。
然后修改 conf 目录下的 nginx.conf 文件：
```
server{
    listen 8888;
    server_name  127.0.0.1;
 
    location /{
        proxy_pass 127.0.0.1:8000;
    }
}
```

此时客户端请求 8888 端口，就不会跨域了。

参考资料：
https://juejin.cn/post/7017614708832206878
https://juejin.cn/post/6844904126246027278
https://juejin.cn/post/6844903767226351623
https://www.jianshu.com/p/9a8d793ec52a
https://developer.mozilla.org/zh-CN/docs/Web/HTTP/CORS
公众号前端点线面