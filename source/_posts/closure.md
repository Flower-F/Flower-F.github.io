---
title: js 闭包详解（译）
date: 2022-01-06 16:43:42
tags: [javascript, 翻译]
copyright: true
---
# 什么是闭包
闭包是一个可以访问外部函数作用域的函数，即便外部函数已经运行完成。这意味着闭包可以记住并访问其外部函数的变量和参数，即使在函数完成之后也是如此。
在深入闭包之前，我们首先需要理解词法作用域。

# 什么是词法作用域
Javascript 词法作用域是指我们可以获取源代码中变量、函数、对象的物理位置。

比如：
```js
let a = 'global';
function outer() {
    let b = 'outer';
    function inner() {
        let c = 'inner'
        console.log(c);   // prints 'inner'
        console.log(b);   // prints 'outer'
        console.log(a);   // prints 'global'
    }
    console.log(a);     // prints 'global'
    console.log(b);     // prints 'outer'
    inner();
}
outer();
console.log(a);  
```
此处函数 `inner` 可以获取它自己作用域、`outer` 函数作用域、全局作用域中的变量；`outer` 函数可以获取它自己作用域、全局作用域中的变量。

# 闭包的具体实例
## Example 1
```js
function person() {
    let name = 'Peter';
    return function displayName() {
        console.log(name);
    };
}
let peter = person();
peter(); // prints 'Peter'
```
在这段代码中，我们调用了 `person` 函数，它会返回内部的函数 `displayName` 并把这个内部函数存储在变量 `peter` 中。当我们调用函数 `peter` 时，实际上也就是在调用函数 `displayName`，因此 console 结果为 Peter。

但是在函数 `displayName` 里面并不存在一个叫 `name` 的变量，也就意味着这个函数可以以某种方式获取它外部的函数 `person` 中的值，甚至在 `person` 函数返回之后。所以 `displayName` 函数实际上就是一个闭包。

## Example 2
```js
function getCounter() {
    let counter = 0;
    return function () {
        return counter++;
    }
}
let count = getCounter();
console.log(count());  // 0
console.log(count());  // 1
console.log(count());  // 2
```
这次我们把一个通过 `getCounter` 返回的匿名函数存储在一个叫做 `count` 的变量中。因为 `count` 函数是一个闭包，它可以通过函数 `getCounter` 获取变量 `counter` 的值，即便是在 `getCounter` 函数已经返回之后。

**这里我们需要注意的是，变量 `counter` 在每次调用时，并没有重置为 0。**这似乎有悖我们之前对函数的认知。
这是因为每次调用函数 `count` 的时候，虽然都会创建一个 `count` 的新的函数作用域，但是对于函数 `getCounter` 始终只存在一个作用域，而 `counter` 变量是在 `getCounter` 的作用域中定义的，所以每次我们调用函数 `count` 的时候，计数会递增，而不是重置为 0。

# 闭包的工作原理
要想清楚地理解闭包的工作原理，我们需要首先理解 Javascript 中的两个重要概念：
- 执行上下文
- 词法环境

## 执行上下文
简单来说，执行上下文是关于 Javascript 代码解析和执行的环境的抽象概念。JavaScript 中运行任何的代码都是在执行上下文中运行的。

因为 Javascript 是单线程语言，所以一次只能有一个正在运行的执行上下文，它由一个被称为执行栈或调用栈的数据结构管理。

当前运行的执行上下文将始终位于栈顶，并且当当前函数运行完成时，对应的执行上下文会从栈顶弹出，并将控制权移交给下一个执行上下文。

请看例子：
![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/20220106201705.png)

当这段代码运行时，Javascript 引擎会创建一个全局执行上下文来运行全局代码，而当遇到函数 `first` 时，它会为 `first` 函数创建一个新的执行上下文，并将其压入栈内。

执行栈的图示就像这样：
![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/20220106201922.png)

当 `first` 函数执行完成时，它的执行上下文会从执行栈中弹出，然后控制权移交给它下面的执行上下文，也就是全局上下文。

## 词法环境
每当 Javascript 引擎为全局代码或者某个函数创建一个新的执行上下文的时候，它同时也会创建一个新的词法环境，用于存储函数执行过程中出现的变量。

词法环境是一个类似 `(标识符, 变量)` 的映射数据结构，这里的标识符具体指变量名或者函数名，而变量指的是对象的引用，包括它的函数类型或者初始值。

每一个词法环境有三个组件：
- 环境记录器：环境记录器用于存储变量或函数声明的实际位置
- 一个对外部环境的引用：外部引用意味着它可以访问它外部（父级）的词法环境
- this 的绑定

词法环境可以被抽象地表示为：

```
lexicalEnvironment = {
    environmentRecord: {
        <identifier>: <value>,
        <identifier>: <value>
    }
    outer: <Reference to the parent lexical environment>
}
```

以下面这段代码为例：

```js
let a = 'Hello World!';
function first() {
    let b = 25;  
    console.log('Inside first function');
}
first();
console.log('Inside global execution context');
```

全局作用域的词法环境如下所示：

```
globalLexicalEnvironment = {
    environmentRecord: {
        a: 'Hello World!',
        first: <reference to function object>
    },
    outer: null
}
```

`first` 函数的词法环境如下所示：

```
functionLexicalEnvironment = {
    environmentRecord: {
        b: 25,
    },
    outer: <globalLexicalEnvironment>
}
```

函数的外部词法环境被设置为全局词法环境，因为该函数被源代码中的全局作用域包围。

注意：当函数执行完成时，它的执行上下文将从执行栈中弹出，但是它的词法环境**不一定**从内存中删除，这取决于该词法环境是否被它外部词法环境属性中的任意其它的词法环境所引用。

# 结合实例来看闭包的工作原理
```js
function person() {
    let name = 'Peter';
    return function displayName() {
        console.log(name);
    };
}
let peter = person();
peter(); // prints 'Peter'
```

当 `person` 函数执行的时候，Javascript 引擎会为这个函数创建一个新的**执行上下文**和**词法环境**。在这个函数执行完成后，它会返回 `displayName` 函数然后将它赋值给变量 `peter`。
词法环境如下所示：

```
personLexicalEnvironment = {
    environmentRecord: {
        name: 'Peter',
        displayName: <displayName function reference>
    },
    outer: <globalLexicalEnvironment>
}
```

当函数 `person` 执行完成之后，执行上下文会从执行栈中弹出。但是它的词法环境依然在内存中，因为它的词法环境被它内部的 `displayName` 函数的词法环境引用了。所以它的变量在内存中依然可以获取。

**简单来说就是，执行上下文删除了，但是词法环境还在。**（译者注）

当 `peter` 函数（实际上是对 `displayName` 函数的引用）执行时，JavaScript 引擎为该函数创建一个新的执行上下文和词法环境。对应的词法环境如下所示：

```
displayNameLexicalEnvironment = {
    environmentRecord: {
        
    }
    outer: <personLexicalEnvironment>
}
```

由于 `displayName` 函数中没有变量，所以它的环境记录器将为空。在执行此函数期间，JavaScript 引擎将尝试在函数的词法环境中查找变量 `name`。

而在 `displayName` 函数的词法环境中没有变量 `name`，所以它会去外部词法环境中找，也就是还在内存中的 `person` 函数的词法环境。JavaScript 引擎查找到了变量 `name` 后，将其打印到控制台。

原文：
https://blog.bitsrc.io/a-beginners-guide-to-closures-in-javascript-97d372284dda