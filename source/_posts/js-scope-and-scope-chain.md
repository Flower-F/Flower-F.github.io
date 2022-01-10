---
title: js 作用域和作用域链详解（译）
date: 2022-01-10 14:30:46
tags: [javascript, 翻译]
copyright: true
---
# 什么是作用域
JavaScript 中的作用域是指变量的可访问性或可见性。也就是说，程序的哪些部分可以访问变量。

# 作用域的重要性
- 作用域的主要好处是具有安全性。也就是说，变量只能从程序的某些区域访问。通过作用域，我们可以避免对程序其他部分的变量进行意外的修改。
- 作用域可以减小命名空间的冲突。也就是说，在不同的作用域中，我们可以使用同名的变量。

# 作用域的种类
作用域有三种类型：
- 全局作用域
- 函数作用域
- 块级作用域

## 全局作用域
不在任何函数或块（一对花括号）内的变量都在全局作用域内。全局作用域内的变量可以从程序中的任何位置访问。例如：
```js
var greeting = 'Hello World!';
function greet() {
    console.log(greeting);
}
// Prints 'Hello World!'
greet();
```

## 函数作用域
函数中声明的变量在函数作用域内，这些变量只能从该函数的内部访问，不能从外部区域访问。例如：
```js
function greet() {
    var greeting = 'Hello World!';
    console.log(greeting);
}
// Prints 'Hello World!'
greet();
// Uncaught ReferenceError: greeting is not defined
console.log(greeting);
```

## 块级作用域
对于 ES6 的 let 和 const 关键字声明的变量，不像 var 声明的变量，它们可以通过最近的一对花括号形成块级作用域。也就是说，在花括号外部不能访问花括号里面声明的变量。例如：
```js
{
    let greeting = 'Hello World!';
    var lang = 'English';
    console.log(greeting); // Prints 'Hello World!'
}
// Prints 'English'
console.log(lang);
// Uncaught ReferenceError: greeting is not defined
console.log(greeting);
```

# 作用域的嵌套
就像 JavaScript 的函数一样，一个作用域可以被另一个作用域包裹。例如：
```js
var name = 'Peter';
function greet() {
    var greeting = 'Hello';
    {
        let lang = 'English';
        console.log(`${lang}: ${greeting} ${name}`);
    }
}
greet();
```
这里有三个彼此嵌套的作用域，包括前面所说的全局作用域、块级作用域、函数作用域。

# 词法作用域
词法作用域（又称静态作用域）的字面意思是在词法分析时（通常称为编译）而不是在运行时确定作用域。例如：
```js
let number = 42;
function printNumber() {
    console.log(number);
}
function log() {
    var number = 54;
    printNumber();
}
// Prints 42
log();
```
这里 `console.log(number)` 会始终打印出 42，不管函数 `printNumber` 在什么时候被调用。这与具有动态作用域的语言不同，支持动态作用域的语言中函数的执行结果可能会受其它函数调用的影响。比如上面的代码中，如果是用一种支持动态作用域的语言编写的话，那么 `console.log(number)` 的输出结果应该是 54。
使用词法作用域，我们可以通过查看源代码来确定变量的作用域。然而，在动态作用域的情况下，只有执行代码才能确定作用域。
大多数编程语言支持静态作用域，如C、C++、java、JavaScript。Perl 支持静态和动态两种作用域。

# 什么是作用域链
在 JavaScript 中使用变量时，JavaScript 引擎将尝试在当前作用域内查找变量的值。如果找不到变量，它将查看外部的作用域，并将继续这样做，直到找到变量或到达全局作用域。
如果仍然找不到变量，它将在全局作用域内隐式声明变量（如果不是在严格模式下），或者返回错误（严格模式下）。
```js
let foo = 'foo';
function bar() {
    let baz = 'baz';
    // Prints 'baz'
    console.log(baz);
    // Prints 'foo'
    console.log(foo);
    number = 42;
    console.log(number);  // Prints 42
}
bar();
console.log(number); // 42
```

当函数 `bar` 被执行时，JavaScript 引擎会去寻找变量 `baz`，然后在当前作用域找到了。接着，它会去寻找变量 `foo`，然后发现无法在当前作用域中找到。所以它会去外部的作用域中寻找，然后成功找到了该变量。
之后，JavaScript 会在当前作用域和外部作用域中寻找 `number` 变量，但是发现找不到。此时如果是非严格模式，会在全局声明一个变量 `number` 并给它赋值 42；若是在严格模式下，将会报错。
总而言之，当一个变量被使用的时候，JavaScript 引擎会沿着作用域链去遍历这个变量直到找到或到达全局作用域。

# 作用域和作用域链的工作原理
为了理解作用域和作用域链的作用原理，我们必须先理解词法环境的概念。

## 什么是词法环境
词法环境是用于保存 `标识符-变量` 的映射的结构。这里标识符是指变量/函数的名称，变量是对实际对象（包括函数对象和数组对象）或基本类型的引用。

简而言之，**词法环境是存储变量和对象引用的地方**。

注意：不要混淆词法作用域和词法环境，词法作用域是在编译时确定的一个范围，词法环境是在程序执行期间存储变量的地方。

一个词法环境可以被抽象地表示如下：
```
lexicalEnvironment = {
    a: 25,
    obj: <ref. to the object>
}
```
当词法作用域中的代码执行时，每个词法作用域中会创建一个新的词法环境。词法环境也有对外部词法环境（即外部作用域）的引用。例如：
```
lexicalEnvironment = {
    a: 25,
    obj: <ref. to the object>
    outer: <outer lexical environemt>
}
```

## JavaScript 引擎如何进行变量的查找
现在我们知道了什么是作用域、作用域链和词法环境，下面让我们了解 JavaScript 引擎如何使用词法环境来确定作用域和作用域链。以下面代码为例：
```js
let greeting = 'Hello';
function greet() {
    let name = 'Peter';
    console.log(`${greeting} ${name}`);
}
greet();
{
    let greeting = 'Hello World!'
    console.log(greeting);
}
```

加载上述脚本时，将创建一个全局词法环境，其中包含在全局作用域内定义的变量和函数。如下所示：
```
globalLexicalEnvironment = {
    greeting: 'Hello'
    greet: <ref. to greet function>
    outer: <null>
}
```
外部作用域设置为 null 的原因是没有全局作用域外部没有别的作用域了。

此后遇到了一个 `greet` 函数的调用。所以 `greet` 函数的词法环境会被创建。如下所示：
```
functionLexicalEnvironment = {
    name: 'Peter'
    outer: <globalLexicalEnvironment>
}
```

此后，JavaScript 引擎会执行 `console.log(${greeting} ${name})` 语句。
JavaScript 引擎尝试在函数的词法环境中查找变量 `greeting` 和 `name`。它在当前词法环境中找到了 `name` 变量，但在当前词法环境中找不到 `greeting` 变量。

所以它去外部的词法环境（此处指全局词法环境）寻找 `greeting` 变量，并成功找到。

接下来，JavaScript 引擎要执行块内的代码。因此，它为该块创建了一个新的词法环境。如下所示：
```
blockLexicalEnvironment = {
    greeting: 'Hello World',
    outer: <globalLexicalEnvironment>
}
```
接着 `console.log(greeting)` 语句被执行，JavaScript 引擎会在当前此法环境找到 `greeting` 变量。

注意：新的词法环境只会被 `let` 和 `const` 声明创建，而不会被 `var` 声明创建。`var` 声明会被添加到当前的词法环境中（全局词法环境或函数词法环境）

总而言之，当在程序中使用变量时，JavaScript 引擎将尝试在当前词法环境中查找该变量，如果在当前词法环境中找不到该变量，它将在外部词法环境中查找该变量。这就是 JavaScript 引擎执行变量查找的方法。

原文：
https://blog.bitsrc.io/understanding-scope-and-scope-chain-in-javascript-f6637978cf53