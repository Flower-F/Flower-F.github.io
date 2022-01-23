---
title: 字节前端青训营第 7 天
date: 2022-01-23 10:07:57
tags: [webpack]
copyright: true
---
# 为什么需要 Webpack

旧时代的缺点：
- 需要手工控制依赖，如果 js 文件很多，就很难控制；如果代码文件之间有依赖关系

# 什么是 Webpack

- 多份资源文件打包成一个 Bundle，减少 http 请求数
- 支持模块化开发
- 支持高级 JS  特性
- 支持 Babel、Eslint、TS、CoffeeScript、Less、Sass
- 支持模块化处理 CSS、图片等资源文件
- 支持 HMR 和 devServer

# Webpack 使用步骤

安装 webpack 和 webpack-cli

```bash
npm i -D webpack webpack-cli
```

书写配置文件 `webpack.config.js`

```js
const path = require('path');

module.exports = {
  entry: './src/index',
}
```

执行编译命令
```bash
npx webpack
```

打包后：
- 多个 js 文件合成一个 main.js
- `import` 语句会变为 `_webpack_require_`

# Webpack 核心流程

![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/20220123101949.png)

# Webpack 使用

**Webpack 的使用方法基本都围绕配置展开**

配置大致可以分为两类：
- 流程类：指关于前面所说的四个步骤
- 工具类：提供更多的工程化能力，比如说 tree-shaking

## 流程类

1. entry：`entry` `content`
2. Dependencies Lookup: `resolve` `externals`

## 处理 CSS


## 处理 JS

需要接入 Babel，Babel 是一个 JS 转译工具。

```
npm i babel-loader
```

## 处理 HTML

```
npm i -D html-webpack-plugin
```

这个插件的作用就是帮我们自动生成一个 index.html 文件，然后帮我们把产物（如 main.js）插入进去

# HMR

热更新：更新代码后，不需要完整刷新整个页面就能显示更新效果。

# Tree-Shaking

树摇，删除掉一些没有用到的模块。对于像 `lodash` 这样的工具库有奇效。

```js
mode: 'production',
optimization: {
  usedExports: true
}
```

# 缓存

# Sourcemap

# Loader

作用：将非 js 资源转换为 js 资源。
