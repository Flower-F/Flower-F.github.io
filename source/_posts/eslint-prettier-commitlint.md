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

修改 package.json，在 lint-staged 配置项中增加 ts 和 tsx 选项

![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/20220123222007.png)

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
```js
types = {
    feat: {
        description: 'A new feature',
        title: 'Features',
        emoji: '✨',
    },
    fix: {
        description: 'A bug fix',
        title: 'Bug Fixes',
        emoji: '🐛',
    },
    docs: {
        description: 'Documentation only changes',
        title: 'Documentation',
        emoji: '📚',
    },
    style: {
        description:
            'Changes that do not affect the meaning of the code (white-space, formatting, missing semi-colons, etc)',
        title: 'Styles',
        emoji: '💎',
    },
    refactor: {
        description:
            'A code change that neither fixes a bug nor adds a feature',
        title: 'Code Refactoring',
        emoji: '📦',
    },
    perf: {
        description: 'A code change that improves performance',
        title: 'Performance Improvements',
        emoji: '🚀',
    },
    test: {
        description: 'Adding missing tests or correcting existing tests',
        title: 'Tests',
        emoji: '🚨',
    },
    build: {
        description:
            'Changes that affect the build system or external dependencies (example scopes: gulp, broccoli, npm)',
        title: 'Builds',
        emoji: '🛠',
    },
    ci: {
        description:
            'Changes to our CI configuration files and scripts (example scopes: Travis, Circle, BrowserStack, SauceLabs)',
        title: 'Continuous Integrations',
        emoji: '⚙️',
    },
    chore: {
        description: "Other changes that don't modify src or test files",
        title: 'Chores',
        emoji: '♻️',
    },
    revert: {
        description: 'Reverts a previous commit',
        title: 'Reverts',
        emoji: '🗑',
    },
}
```