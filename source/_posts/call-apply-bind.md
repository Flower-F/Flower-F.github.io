---
title: call、apply、bind 辨析（译）
date: 2022-01-10 19:58:25
tags:  [javascript, 翻译]
copyright: true
---
# this
如果您正在学习 JavaScript，您可能已经了解到了 `this` 关键字。JavaScript 中的 `this` 关键字的行为与其他编程语言不同，这给不少程序员带来了困惑。
在其他面向对象编程语言中，`this` 关键字总是指向类的当前实例。而在 JavaScript 中，它的值取决于函数的调用方式。
我们来通过一些例子说明一下 JavaScript 中的 `this`。

## Example1
```js
const person = {
    firstName: 'John',
    lastName: 'Doe',
    printName: function() {
        console.log(this.firstName + ' ' + this.lastName);
    }
};

person.printName(); // John Doe
```
此处因为我通过 `person` 对象来调用 `printName` 函数，所以 `this` 指向 `person` 对象。

我们在刚才的代码片段后面添加下面两行代码：
```js
const printFullName = person.printName;
printFullName(); // undefined undefined
```
我们惊奇地发现打印结果是两个 `undefined`。

**原因是什么？**
此处我们把 `person.printName` 的引用存储到了变量 `printFullName` 中。此后，我们调用它，且没有指明调用它的对象。这种情况下，`this` 会指向 window 或 undefined（严格模式下）。

因此会输出两个 undefined；若是在严格模式下则会报错。

## Example 2
```js
const counter = {
    count: 0,
    incrementCounter: function() {
        console.log(this);
        this.count++;
    }
}
document.querySelector('.btn').addEventListener('click', counter.incrementCounter);
```

`incrementCounter` 中的 `this` 将会指向谁呢？
事实上，在上面的代码中，this 关键字会指向 `event` 对象，而不是 `counter` 对象。

根据前面所举的例子，我们可以发现函数中的 `this` 关键字指向根据函数的调用情况决定。有时候我们会一不小心丢失掉 `this`。那么我们如何才能防止这种情况的发生呢？

# call、bind、apply
我们知道在 JavaScript 中函数是一种特殊的对象，所以我们可以获取它的方法和属性。为了证明函数也是对象，我们可以做一些诸如下面这样的操作：
```js
function greeting() {
    console.log('Hello World');
}
greeting.lang = 'English';
// Prints 'English'
console.log(greeting.lang);
```
JavaScript 也提供了一些特殊的方法和属性给每一个函数对象。所以 JavaScript 中的每个函数都继承了一些方法，其中就包括 call、apply、bind。

# bind
bind 创建一个新的函数，并把 this 指向传入的对象。
语法为：
```js
function.bind(thisArg, arg1, arg2, ...)
```

举个例子，假如我们有两个人物对象。
```js
const john = {
    name: 'John',
    age: 24,
};
const jane = {
    name: 'Jane',
    age: 22,
};

function greeting() {
  console.log(`Hi, I am ${this.name} and I am ${this.age} years old`);
}
```
我们可以使用 `bind` 方法来让 `this` 指向 john 和 jane 对象。
```js
const greetingJohn = greeting.bind(john);
// Hi, I am John and I am 24 years old
greetingJohn();
const greetingJane = greeting.bind(jane);
// Hi, I am Jane and I am 22 years old
greetingJane();
```

此处 `greeting.bind(john)` 创建了一个新的函数，且 `this` 指向了传入的 john 对象，接着把它赋值给了变量 `greetingJohn`

我们也可以使用 bind 来 DOM 操作的情况，比如：
```js
const counter = {
    count: 0,
    incrementCounter: function() {
        console.log(this);
        this.count++;
    }
}
document.querySelector('.btn').addEventListener('click', counter.incrementCounter.bind(counter));
```
在上面的例子中，this 会正确地指向 `counter` 对象而不是 `event` 对象。

## bind 接收多个参数
`bind` 可以接收多个参数。

例如：
```js
function greeting(lang) {
    console.log(`${lang}: I am ${this.name}`);
}
const john = {
    name: 'John'
};
const jane = {
    name: 'Jane'
};
const greetingJohn = greeting.bind(john, 'en');
greetingJohn();
const greetingJane = greeting.bind(jane, 'es');
greetingJane();
```

在上面的例子中，我们把参数 `en` 传递给了 `greeting` 函数。

# call
`call` 会将 `this` 指向对象，并立即执行。
`call` 和 `bind` 的区别在于，`call` 会使函数立即执行，而 `bind` 会创建一个新的函数，不会马上执行。

语法：
```js
function.call(thisArg, arg1, arg2, ...)
```

举个例子：
```js
function greeting() {
    console.log(`Hi, I am ${this.name} and I am ${this.age} years old`);
}
const john = {
    name: 'John',
    age: 24,
};
const jane = {
    name: 'Jane',
    age: 22,
};
// Hi, I am John and I am 24 years old
greeting.call(john);
// Hi, I am Jane and I am 22 years old
greeting.call(jane);
```
上面例子我们可以看出 `call` 的结果是立即执行该函数。

## call 接收多个参数
`call` 可以接收多个参数。

例如：
```js
function greet(greeting) {
    console.log(`${greeting}, I am ${this.name} and I am ${this.age} years old`);
}
const john = {
    name: 'John',
    age: 24,
};
const jane = {
    name: 'Jane',
    age: 22,
};
// Hi, I am John and I am 24 years old
greet.call(john, 'Hi');
// Hello, I am Jane and I am 22 years old
greet.call(jane, 'Hello');
```

# apply
`apply` 和 `call` 非常相像，不同之处在于 `apply` 的参数是一个数组，而 `call` 的参数是分散开的。

示例如下：
```js
function greet(greeting, lang) {
    console.log(lang);
    console.log(`${greeting}, I am ${this.name} and I am ${this.age} years old`);
}
const john = {
    name: 'John',
    age: 24,
};
const jane = {
    name: 'Jane',
    age: 22,
};
// Hi, I am John and I am 24 years old
greet.apply(john, ['Hi', 'en']);
// Hello, I am Jane and I am 22 years old
greet.apply(jane, ['Hello', 'es']);
```

原文：
https://blog.bitsrc.io/understanding-call-bind-and-apply-methods-in-javascript-33dbf3217be