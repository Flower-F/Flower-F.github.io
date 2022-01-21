---
title: 字节前端青训营第 5 天（下）
date: 2022-01-20 14:00:28
tags: [nodejs]
copyright: true
---
# 应用场景
- 前端工程化
- 服务端
- Electron 跨端桌面应用

# 编写 Http Server

## Hello World
```js
const http = require('http');

const port = 8000;

const server = http.createServer((req, res) => {
  res.end('hello world');
})

server.listen(port, () => {
  console.log(`Listening on port ${port}`);
})
```

## JSON
```js
// 接收请求
const http = require('http');

const port = 8000;

const server = http.createServer((req, res) => {
  const bufs = [];
  req.on('data', (buf) => {
    bufs.push(buf);
  })
  req.on('end', () => {
    const buf = Buffer.concat(bufs).toString('utf-8');
    try {
      const result = JSON.parse(buf);
      const msg = result.msg || 'success';
      const responseMsg = {
        msg: `receive: ${msg}`
      }
      res.setHeader('Content-Type', 'application/json');
      res.end(JSON.stringify(responseMsg));
    } catch (err) {
      res.end('invalid json data');
    }
  })
})

server.listen(port, () => {
  console.log(`Listening on port ${port}`);
})
```

```js
// 发请求
const http = require('http');

const body = JSON.stringify({
  msg: 'Hello Byte Dance'
})

const req = http.request('http://127.0.0.1:8000', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
  }
}, res => {
  const bufs = [];
  res.on('data', buf => {
    bufs.push(buf);
  })
  res.on('end', () => {
    const buf = Buffer.concat(bufs);
    const json = JSON.parse(buf);

    console.log('json msg is: ', json.msg);
  })
})

req.end(body);
```

## 静态文件服务
```js
const http = require('http');
const fs = require('fs');
const url = require('url');
const path = require('path');

const port = 8000;

const folderPath = path.resolve(__dirname, './static');

const server = http.createServer((req, res) => {
  const info = url.parse(req.url);
  const filePath = path.resolve(folderPath, '.' + info.pathname);
  const fileStream = fs.createReadStream(filePath);

  fileStream.pipe(res);
})

server.listen(port, () => {
  console.log(`Listening on port ${port}`);
})
```

## SSR
```js
const http = require('http');
const React = require('react');
const ReactDomServer = require('react-dom/server')

const port = 8000;

function App() {
  return React.createElement('h1', {
    children: 'Hello SSR'
  });
}

const server = http.createServer((req, res) => {
  res.end(`
    <!DOCTYPE html>
    <html lang="en">
    
    <head>
      <meta charset="UTF-8">
      <meta http-equiv="X-UA-Compatible" content="IE=edge">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <title>Nodejs</title>
    </head>
    
    <body>
      ${ReactDomServer.renderToString(
        React.createElement(App)
      )}
    </body >
    
    </html>
  `
  )
})

server.listen(port, () => {
  console.log(`Listening on port ${port}`);
})
```

# 部署
- 守护进程：当进程退出以后
- 多进程：通过 cluster 模块便捷地利用多进程
- 记录进程状态：用于诊断