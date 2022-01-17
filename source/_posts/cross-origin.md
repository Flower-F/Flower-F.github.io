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
- `<iframe>`

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
JSONP 工作原理：在网页有一些标签天生就具有跨域能力，比如 `img` `link` `iframe` `script` 等。JSONP 就是利用 `script` 标签的跨域能力来发送请求的。

![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/20220117230214.png)
举个例子，客户端传入 a 和 b，服务端传回 a + b 的结果。

```js
// 服务端 http://127.0.0.1:8000
const http = require('http');
const urllib = require('url');

const port = 8000;

http.createServer((req, res) => {
    const { query } = urllib.parse(req.url, true);
    const { a, b, callback } = query;
    const ans = parseInt(a) + parseInt(b);
    res.end(`${callback}('${ans}')`);
}).listen(port, function () {
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

# WebSocket

参考资料：
https://juejin.cn/post/7017614708832206878
https://juejin.cn/post/6844904126246027278
https://juejin.cn/post/6844903767226351623
https://www.jianshu.com/p/9a8d793ec52a
公众号前端点线面