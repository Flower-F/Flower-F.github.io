---
title: 使用 Eslint、Prettier、Commitlint 打造 React 开发规范
date: 2022-01-12 23:39:32
tags: [react]
copyright: true
---
# 创建应用
首先使用脚手架 `create-react-app` 创建应用
```bash
yarn create react-app project-name --template typescript
```

# 添加 baseUrl
打开文件 tsconfig.json，添加 baseUrl

![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/20220112234515.png)

# 添加 prettier
[prettier 官方文档](https://prettier.io/docs/en/index.html)

```bash
yarn add --dev --exact prettier
```

在根目录创建空文件 **.prettierrc.json**，里面写入内容
```json
{}
```

根目录创建新文件 **.prettierignore**，里面写入内容
```json
build
coverage
```

接下来配置 **pre-commit hooks**
```bash
npx mrm@2 lint-staged
```
配置后，每次代码提交之前都会自动格式化

为了避免 prettier 与 eslint 冲突，需要安装 **eslint-config-prettier**
```bash
yarn add eslint-config-prettier -D
```

修改 package.json，在 lint-staged 配置项中增加 ts 和 tsx 选项（可自行选择其余拓展名）

![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/20220123222007.png)

修改 package.json，添加 prettier 拓展。

![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/20220125103608.png)

# 添加 commitlint
[commitlint github 链接](https://github.com/conventional-changelog/commitlint)

设置提交规范
```bash
yarn add @commitlint/config-conventional @commitlint/cli -D
```

根目录新建文件 **commitlint.config.js**，添加如下内容
```js
module.exports = { extends: ['@commitlint/config-conventional'] }
```

运行命令
```bash
yarn husky add .husky/commit-msg 'yarn commitlint --edit $1'
```

提交类型总结
- build	编译相关的修改，例如发布版本、对项目构建或者依赖的改动
- chore	其他修改, 比如改变构建流程、或者增加依赖库、工具等
- ci	持续集成修改
- docs	文档修改
- feat	新特性、新功能
- fix	修改bug
- perf	优化相关，比如提升性能、体验
- refactor	代码重构
- revert	回滚到上一个版本
- style	代码格式修改, 注意不是 css 修改
- test	测试用例修改

参考资料：
链接：https://www.jianshu.com/p/9edce0b60f83