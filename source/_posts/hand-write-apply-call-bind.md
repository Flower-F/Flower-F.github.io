---
title: 手写实现 call、apply、bind
date: 2022-01-10 23:28:46
tags: [javascript, 手写]
copyright: true
---
# call
- 获取第一个参数，若不存在则为 window
- 执行 & 删除这个函数
- 指定 this 到函数并传入给定参数执行函数
- 如果不传入参数，默认指向为 window

```js
Function.prototype.myCall = function (context, ...args) {
    // 若第一个参数传入 null 或 undefined，this 指向 window
    if (context === undefined || context === null) {
        context = window;
    }
    // 在 context 上加一个唯一值，避免影响 context 上的属性
    const key = Symbol('key');
    // 此处的 this 指的是传入的函数
    context[key] = this;
    // 调用函数
    const result = context[key](...args);
    // 删除副作用，避免 context 的属性越来越多
    delete context[key];

    return result;
}

function f(a, b) {
    console.log(a, b)
    console.log(this.name)
}
let obj = {
    name: '张三'
}
f.myCall(obj, [1, 2]);
```

# apply
apply 与 call 几乎完全一样，只是传入的参数形式不同
```js
Function.prototype.myApply = function (context, args) {
    // 若第一个参数传入 null 或 undefined，this 指向 window
    if (context === undefined || context === null) {
        context = window;
    }
    // 在context上加一个唯一值，避免影响 context 上的属性
    const key = Symbol('key');
    // 此处的 this 指的是传入的函数
    context[key] = this;
    // 调用函数
    const result = context[key](...args);
    // 删除副作用，避免 context 的属性越来越多
    delete context[key];

    return result;
}

function f(a, b) {
    console.log(a, b)
    console.log(this.name)
}
let obj = {
    name: '张三'
}
f.myApply(obj, [1, 2]);
```

# bind
```js
Function.prototype.myBind = function (context, ...args) {
    if (typeof (this) !== 'function') {
        throw new TypeError('The bound object needs to be a function');
    }
    const _this = this;

    return function newFunction() {
        // 使用了 new
        if (this instanceof newFunction) {
            return new _this(...args);
        } else {
            return _this.call(context, ...args);
        }
    }
}

let a = {
    name: 'poetries',
    age: 12
}
function foo(a, b) {
    console.log(this.name);
    console.log(this.age);
    console.log(a);
    console.log(b);
}
foo.myBind(a, 1, 2)(); // => 'poetries'
```