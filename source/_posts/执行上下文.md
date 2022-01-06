---
title: js 执行上下文详解（译）
date: 2022-01-06 12:46:33
tags: [javascript]
copyright: true
---
# 什么是执行上下文
简单来说，执行上下文是关于 Javascript 代码解析和执行的**环境**的抽象概念。JavaScript 中运行任何的代码都是在执行上下文中运行的。

# 执行上下文的类型
- 全局执行上下文：任何不在函数内部的代码都在全局上下文中。它会执行两件事：创建一个全局的 window 对象（浏览器的情况下），并且设置 this 的值等于这个全局对象。一个程序中只会有一个全局执行上下文。
- 函数执行上下文：每当一个**函数被调用**时, 都会为该函数创建一个新的上下文。每一个函数都有它自己的执行上下文，但它是在函数被调用的时候创建的。函数执行上下文可以有任意多个。
- eval 函数执行上下文 — 执行在 eval 函数内部的代码也会有它属于自己的执行上下文，但 JavaScript 开发者并不经常使用 eval，所以此处不再赘述。

# 执行栈
执行栈，也就是调用栈，它用来存储代码运行时创建的所有执行上下文。本质上它就是数据结构中所说的栈，满足先进后出。
当 JavaScript 引擎第一次读取你的脚本时，它会创建一个全局执行上下文并将其压入执行栈；每当引擎遇到一个函数调用，它会为该函数创建一个新的执行上下文并压入栈。
JavaScript 引擎会优先运行执行上下文位于栈顶的函数。当该函数运行结束时，其对应的执行上下文会从栈中弹出，上下文的控制权将被移到当前执行栈的下一个执行上下文。

示例：
```js
let a = 'Hello World!';
function first() {
    console.log('Inside first function');
    second();
    console.log('Again inside first function');
}
function second() {
    console.log('Inside second function');
}
first();
console.log('Inside Global Execution Context');
```
![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/20220106131015.png)

- 当上述代码在浏览器加载时，JavaScript 引擎创建了一个全局执行上下文并把它压入当前执行栈。当遇到 `first()` 函数调用时，JavaScript 引擎为该函数创建一个新的执行上下文并把它压入当前执行栈
- 当从 `first()` 函数内部调用 `second()` 函数时，JavaScript 引擎为 `second()` 函数创建了一个新的执行上下文并把它压入当前执行栈的顶部。当 `second()` 函数执行完毕，它的执行上下文会从当前栈弹出，并且控制权移交给下一个执行上下文，即 `first()` 函数的执行上下文
- 当 `first()` 执行完毕，它的执行上下文从栈弹出，上下文控制权会被移交给全局执行上下文。一旦所有代码执行完毕，JavaScript 引擎从当前栈中移除全局执行上下文

# 执行上下文是如何创建的
创建执行上下文有两个阶段：
- 创建阶段
- 执行阶段

## 创建阶段
在创建阶段创建执行上下文。在创建阶段会发生以下情况：
1. 创建词法环境组件
2. 创建变量环境组件

我们可以将其抽象表示为：
```js
ExecutionContext = {
    LexicalEnvironment = <ref. to LexicalEnvironment in memory>,
    VariableEnvironment = <ref. to VariableEnvironment in  memory>,
}
```

## 词法环境
根据官方的 ES6 文档，一个词法环境由**环境记录器**和一个可能为空的**对于外部环境的引用**组成。

示例：
```js
var a = 20;
var b = 40;
function foo() {
     console.log('bar');
}
```

对应的词法环境为：
```
lexicalEnvironment = {
    a: 20,
    b: 40,
    foo: <ref. to foo function>
}
```

每一个词法环境有三个组件：
- 环境记录器
- 一个对外部环境的引用
- this 的绑定

### 环境记录器
环境记录用于在词法环境中存储变量和函数声明的位置。
环境记录器有两种类型：
- 声明式环境记录器：顾名思义，它存储变量和函数声明。**函数代码**的词法环境包含声明性环境记录
- 对象环境记录器：**全局代码**的词法环境包含一个对象环境记录器。除了变量和函数声明外，对象环境记录器还存储全局对象 window。因此，对于每个绑定对象的属性（对于浏览器，它包含浏览器提供给 window 的属性和方法），将会创建一条新的记录

注意：对于函数代码，环境记录器还包含一个 arguments 对象。

示例：
```js
function foo(a, b) {
  var c = a + b;
}
foo(2, 3);
// argument object
Arguments: {0: 2, 1: 3, length: 2}
```

### 对于外部环境的引用
对外部环境的引用意味着它可以访问外部的词法环境。这意味着，如果是当前词法环境中找不到的变量，JavaScript 引擎可以在外部环境中查找这些变量。

### this 的绑定
在全局执行上下文中，this 的值引用全局对象。（在浏览器中，这指的是窗口对象）
在函数执行上下文中，其值取决于函数的调用方式。如果它由对象的引用调用，则此值将设置为该对象，否则，此值将设置为全局对象或 undefined（严格模式）。

示例：
```js
const person = {
    name: 'peter',
    birthYear: 1994,
    calcAge: function() {
        console.log(2018 - this.birthYear);
    }
}
person.calcAge(); 
// 'this' refers to 'person', because 'calcAge' was called with //'person' object reference
const calculateAge = person.calcAge;
calculateAge();
// 'this' refers to the global window object, because no object reference was given
```

词法环境大概如下：
```
GlobalExectionContext = {
    LexicalEnvironment: {
        EnvironmentRecord: {
            Type: "Object",
                // Identifier bindings go here
            }
            outer: <null>,
            this: <global object>
        }
    }
    FunctionExectionContext = {
        LexicalEnvironment: {
            EnvironmentRecord: {
                Type: "Declarative",
                // Identifier bindings go here
            }
            outer: <Global or outer function environment reference>,
            this: <depends on how function is called>
        }
}
```

## 变量环境
**它也是一个词法环境**，其环境记录器保存在此执行上下文中由 VariableStatements 创建的绑定。
因为变量环境也是一个词法环境，所以它具有上面定义的词法环境的所有属性和组件。
在 ES6 中，词法环境组件和变量环境组件之间的一个区别是前者用于存储函数声明和变量（let 和 const）绑定，而后者仅用于存储变量（var）绑定。

# 执行阶段
在这个阶段，所有的变量赋值都完成了，代码最终会被执行。

示例：
```js
let a = 20;
const b = 30;
var c;
function multiply(e, f) {
    var g = 20;
    return e * f * g;
}
c = multiply(20, 30);
```

当上面的代码被执行时，Javascript 引擎会创建一个全局执行上下文来执行全局代码：
```
GlobalExectionContext = {
    LexicalEnvironment: {
        EnvironmentRecord: {
            Type: "Object",
            // Identifier bindings go here
            a: < uninitialized >,
            b: < uninitialized >,
            multiply: < func >
        }
        outer: <null>,
        ThisBinding: <Global Object>
    },
    VariableEnvironment: {
        EnvironmentRecord: {
            Type: "Object",
            // Identifier bindings go here
            c: undefined,
        }
        outer: <null>,
        ThisBinding: <Global Object>
    }
}
```

当函数 `multiply(20, 30)` 被调用时，一个新的函数执行上下文会被创建来执行函数代码：
```
FunctionExectionContext = {
    LexicalEnvironment: {
        EnvironmentRecord: {
            Type: "Declarative",
            // Identifier bindings go here
            Arguments: {0: 20, 1: 30, length: 2},
        },
        outer: <GlobalLexicalEnvironment>,
        ThisBinding: <Global Object or undefined>,
    },
    VariableEnvironment: {
        EnvironmentRecord: {
            Type: "Declarative",
            // Identifier bindings go here
            g: undefined
        },
        outer: <GlobalLexicalEnvironment>,
        ThisBinding: <Global Object or undefined>
    }
}
```

此后，执行上下文将经历执行阶段，也就是要完成对函数内变量的赋值。（此处指对变量 g 的赋值）

```
FunctionExectionContext = {
    LexicalEnvironment: {
        EnvironmentRecord: {
            Type: "Declarative",
            // Identifier bindings go here
            Arguments: {0: 20, 1: 30, length: 2},
        },
        outer: <GlobalLexicalEnvironment>,
        ThisBinding: <Global Object or undefined>,
    },
    VariableEnvironment: {
        EnvironmentRecord: {
            Type: "Declarative",
            // Identifier bindings go here
            g: 20
        },
        outer: <GlobalLexicalEnvironment>,
        ThisBinding: <Global Object or undefined>
    }
}
```

函数执行完成后，返回值会被存储在变量 c 之中。所以全局的词法环境会被更新。最终，全局的代码被执行完成，程序也运行完成。

注意：你也许注意到了，let 和 const 定义的变量在创建阶段没有任何赋值操作，但是 var 定义的变量被赋值为了 undefined。这也就解释了为什么当你想获取已声明但未定义的变量时，var 声明的变量会得到 undefined，而 let 声明的变量会显示 undeclared。这也就是我们所谓的变量提升。

# 总结
在本文中，我们讨论了 JavaScript 程序内部的执行机制。虽然学习这些概念并不是成为一名出色的 JavaScript 开发人员的必要条件，但对上述概念有一个良好的理解将有助于您更轻松、更深入地理解其他概念，如变量提升、作用域和闭包。

参考资料：
本文原文：
https://blog.bitsrc.io/understanding-execution-context-and-execution-stack-in-javascript-1c9ea8642dd0
参考译文：
https://juejin.cn/post/6844903682283143181
https://juejin.cn/post/6844903704466833421