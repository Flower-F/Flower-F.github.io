---
title: js 异步原理详解（译）
date: 2022-01-10 10:49:09
tags: [javascript, 翻译]
copyright: true
---
JavaScript 是一种单线程编程语言，这意味着每次只能做一件事情。也就是说，JavaScript 引擎在每个线程中一次只能处理一条语句。虽然单线程语言简化了代码的编写，因为您不必担心并发性问题，但这也意味着您无法在不阻塞主线程的情况下执行长时间操作，如网络访问。

# JavaScript 同步工作原理
在深入研究异步 JavaScript 之前，让我们首先了解同步 JavaScript 代码在 JavaScript 引擎中是如何执行的。举个例子：
```js
const second = () => {
    console.log('Hello there!');
}
const first = () => {
    console.log('Hi there!');
    second();
    console.log('The End');
}
first();
```
为了理解上面的代码在 JavaScript 引擎中是如何执行的，我们必须要理解执行上下文和调用栈（又称执行栈）的概念

## 执行上下文
执行上下文是一个关于解析和执行 JavaScript 代码的环境的抽象概念。任何代码在 JavaScript 中运行时，实际上都是在执行上下文中运行。函数代码在函数执行上下文中执行，全局代码在全局执行上下文中执行。每个函数都有自己的执行上下文。

## 调用栈
调用栈是一个具有后进先出结构的栈，用于存储代码执行期间创建的所有执行上下文。
JavaScript 只有一个单调栈，因为它是一门单线程编程语言。调用栈具有后进先出结构，这意味着只能从栈顶进行添加或删除操作。
我们重新理解一下上面给出的代码片段：
```js
const second = () => {
    console.log('Hello there!');
}
const first = () => {
    console.log('Hi there!');
    second();
    console.log('The End');
}
first();
```

![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/20220110111059.png)
<center>
    <div style="color:orange;
    display: inline-block;
    color: #888;"><b>上述代码的调用栈</b></div>
    <br>
</center>

# JavaScript 异步工作原理
现在我们已经了解了调用栈的基本概念，以及同步 JavaScript 的工作原理，现在让我们再回到异步 JavaScript。

## 什么是阻塞
现在假设我们的图片加载和网络请求操作是同步进行，就像下面的示例：
```js
const processImage = (image) => {
    /**
    * doing some operations on image
    **/
    console.log('Image processed');
}
const networkRequest = (url) => {
    /**
    * requesting network resource
    **/
    return someData;
}
const greeting = () => {
    console.log('Hello World');
}
processImage(logo.jpg);
networkRequest('www.somerandomurl.com');
greeting();
```
进行图像处理和网络请求需要时间。因此，当调用 `processImage` 函数时，需要花费一些时间，具体取决于图像的大小。
当 `processImage` 函数完成时，它将从调用栈中删除。然后调用 `networkRequest` 函数并将其压入栈中。同样，它的执行也需要一些时间。
最后，当 `networkRequest` 函数完成时，会调用 `greeting` 函数，因为它只包含一条 `console.log` 语句，而且 `console.log` 语句执行得通常很快，因此会接着立即执行 `greeting` 函数。
因此，我们必须等到函数 `processImage` 和 `networkRequest` 完成。这意味着这些函数将会阻塞调用栈，也即是阻塞主线程。因此，在执行上述代码时，我们不能执行任何其他操作，这是不理想的，因为会造成不好的用户体验。

## 解决方案
最简单普遍的一种方法是使用异步的调用栈。我们通过使用异步的调用栈来使得我们的代码不会阻塞。比如：
```js
const networkRequest = () => {
    setTimeout(() => {
        console.log('Async Code');
    }, 2000);
};
console.log('Hello World');
networkRequest();
```
这里我用 `setTimeout` 函数来模拟网络请求。请牢记 `setTimeout` 不是 Javascript 引擎的一部分，而是 web API 的一部分。（在 Nodejs 中，web API 被 C/C++ API 所取代）
为了理解这段代码是如何执行的，我们需要了解更多的相关概念，比如事件循环和任务队列（也叫消息队列）。

![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/20220110114417.png)
<center>
    <div style="color:orange;
    display: inline-block;
    color: #888;"><b>JavaScript 运行时环境的概述</b></div>
    <br>
</center>

事件循环、web API 和任务队列不是 JavaScript 引擎的一部分，而是浏览器 JavaScript 运行时环境或 Nodejs 的 JavaScript 运行时环境的一部分。（在 Nodejs 中，web API 被 C/C++ API 所取代）

现在让我们回到上面的代码，看看它是如何以异步方式执行的。
```js
const networkRequest = () => {
    setTimeout(() => {
        console.log('Async Code');
    }, 2000);
};
console.log('Hello World');
networkRequest();
console.log('The End');
```

当上述代码被加载到浏览器中时，`console.log(‘Hello World’)` 被压入执行栈，并在完成执行后弹出执行栈。接下来，遇到了一个 `networkRequest` 函数的调用，所以它的执行上下文被压入栈中。
接着 `setTimeout`函数被调用了，所以它被压入栈中。`setTimeout` 函数有两个参数，第一个是回调函数，第二个是延迟的时间（单位是 ms）。
`setTimeout` 函数在 web API 环境下开启了一个时长 2s 的定时器。此时，`setTimeout` 函数已经执行完成并从栈中弹出。此后，`console.log('The End')` 被压入调用栈，执行完成后从栈中移出。
接着，当定时器计时时间到达，回调函数被压入到任务队列中。但是任务队列中的回调函数不会被立即执行，这时候事件循环开始起作用了。

## 事件循环
事件循环的任务是查看调用栈并确定调用栈是否为空。如果调用栈为空，它将查看任务队列，查看是否有任何回调函数在等待执行。
在这个例子中，任务队列中有一个回调函数，而且调用栈是空的，所以事件循环会把这个回调函数压入调用栈中，也就是把 `console.log(‘Async Code’)` 压入调用栈，执行并从栈中弹出。此后，全局代码调用结束，全局执行上下文从栈中弹出，然后程序运行完成。

## DOM 事件
任务队列中也包含 DOM 中的事件操作，比如点击事件、键盘事件等。比如：
```js
document.querySelector('.btn').addEventListener('click',(event) => {
    console.log('Button Clicked');
});
```
在这个例子中，事件监听器位于 web API 环境中，并等待某个事件（在本例中为点击事件）发生。当该事件发生时，回调函数会被存储进消息队列中等待执行。事件循环会检查调用栈是否为空，如果为空，则将任务队列中事件的回调函数压入栈中并执行。

## ES6 的微任务队列
ES6 引入了微任务队列的概念，用于 Promise 的相关操作。宏任务队列和微任务队列之间的区别在于，微任务队列的优先级高于宏任务队列，这意味着微任务队列中的回调函数执行将优先于宏任务队列中的回调函数。

举个例子：
```js
console.log('Script start');
setTimeout(() => {
    console.log('setTimeout');
}, 0);
new Promise((resolve) => {
    resolve('Promise resolved');
}).then(res => console.log(res))
    .catch(err => console.log(err));
console.log('Script End');
```
输出如下：
```
Script start
Script End
Promise resolved
setTimeout
```

可见 promise 是先于 `setTimeout` 函数执行的，因为 promise 的 response 会被存储在微任务队列，优先级高于宏任务队列。

我们来看第二个例子：
```js
console.log('Script start');
setTimeout(() => {
    console.log('setTimeout 1');
}, 0);
setTimeout(() => {
    console.log('setTimeout 2');
}, 0);
new Promise((resolve) => {
    resolve('Promise 1 resolved');
}).then(res => console.log(res))
    .catch(err => console.log(err));
new Promise((resolve) => {
    resolve('Promise 2 resolved');
}).then(res => console.log(res))
    .catch(err => console.log(err));
console.log('Script End');
```

输出为
```
Script start
Script End
Promise 1 resolved
Promise 2 resolved
setTimeout 1
setTimeout 2
```

我们可以看到，这两个 promise 是在 `setTimeout` 中的回调函数之前执行的，因为事件循环认为微任务队列中的任务优先于宏任务队列中的任务。
当事件循环执行微任务队列中的任务时，如果在此期间存在一个 promise 已经 resolve 了，它将被添加到同一个微任务队列的末尾，并且无论回调等待执行的时间有多长，它都将在宏任务队列中的回调执行之前执行。

比如下面这段代码：
```js
console.log('Script start');
setTimeout(() => {
    console.log('setTimeout');
}, 0);
new Promise((resolve) => {
    resolve('Promise 1 resolved');
}).then(res => console.log(res));
new Promise((resolve) => {
    resolve('Promise 2 resolved');
}).then(res => {
    console.log(res);
    return new Promise((resolve) => {
        resolve('Promise 3 resolved');
    })
}).then(res => console.log(res));
console.log('Script End');
```

执行结果为
```
Script start
Script End
Promise 1 resolved
Promise 2 resolved
Promise 3 resolved
setTimeout
```
因此，微任务队列中的所有任务都将在宏任务队列中的任务之前执行。也就是说，在执行宏任务队列中的任何回调函数之前，事件循环将首先清空微任务队列。

原文：
https://blog.bitsrc.io/understanding-asynchronous-javascript-the-event-loop-74cd408419ff