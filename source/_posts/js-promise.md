---
title: Promise 详解（译）
date: 2022-01-10 21:00:48
tags: [javascript, 翻译]
copyright: true
---
# 为什么需要 Promise
JavaScript 是一门单线程编程语言，这意味着一次只能发生一件事情。在 ES6 之前，我们使用回调函数来处理异步任务，例如网络请求。使用 promise，我们可以避免臭名昭著的回调地狱，使得我们的代码更简洁、可读性更高、更容易理解。

假设我们要从服务器获取一些异步数据，使用回调函数来处理，我们可能会遇到这种情况：
```js
getData(function(x){
    console.log(x);
    getMoreData(x, function(y){
        console.log(y); 
        getSomeMoreData(y, function(z){ 
            console.log(z);
        });
    });
});
```
这就是回调地狱，每一个回调都被嵌套在另一个回调之中，除了最外层之外，其余所有的回调函数都依赖于它们父级回调函数。

我们可以使用 Promise 重写上面的代码：
```js
getData()
    .then((x) => {
        console.log(x);
        return getMoreData(x);
    })
    .then((y) => {
        console.log(y);
        return getSomeMoreData(y);
    })
    .then((z) => {
        console.log(z);
    });
```

我们可以看到代码变得更简洁、更清晰也更易于理解了。

# 什么是 Promise
Promise 对象用于表示一个异步操作的最终完成（或失败）及其结果值。比如我们需要从服务器获取数据，Promise 承诺（promise）会帮我们在将来使用的时候获取到数据。

在我们讨论技术相关的问题之前，让我们先了解一下 Promise 的相关术语。

# Promise 的状态
Promise 有三种状态：
- pending：初始状态，承诺既没有被兑现，也没有被拒绝。
- resolved / fulfilled：意味着操作成功完成。
- rejected：意味着操作失败。

![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/20220110212112.png)

# Promise 的创建
```js
const promise = new Promise((resolve, reject) => {
    // ...
});
```
我们可以使用 Promise 构造函数来创建对象，该构造函数的参数只有一个，它是一个执行器函数，其中包括两个回调函数 resolve 和 reject。

执行器函数会在 promise 对象创建的时候就直接执行。我们可以使用 resolve 当操作成功，使用 reject 当操作失败。如下所示：

```js
const promise = new Promise((resolve, reject) => {
    if (allWentWell) {
        resolve('All things went well!');
    } else {
        reject('Something went wrong');
    }
});
```

resolve 和 reject 可以传入一个参数，类型为 string、number、boolean、array 或 object。

我们来通过另一个例子彻底弄明白 promise 的创建。
```js
const promise = new Promise((resolve, reject) => {
    const randomNumber = Math.random();
    setTimeout(() => {
        if (randomNumber < 0.6) {
            resolve('All things went well!');
        } else {
            reject('Something went wrong');
        }
    }, 2000);
});
```
在这里，我使用 Promise 构造函数创建一个新的 promise。promise 创建 2 秒后 resolve 或 reject。如果随机数小于 0.6，则 resolved，否则就 rejected。

当 promise 创建的时候，它会位于 pending 状态。
![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/20220110213623.png)

2s 后当定时器结束，根据随机数情况，promise 要么会 resolved，要么会 rejected。
resolved 的情况：
![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/20220110213808.png)
rejected 的情况：
![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/20220110213851.png)

注意：一个 promise 只能被 resolve 或者 reject 一次。多余的 resolve 或 reject 操作将不起作用。比如：
```js
const promise = new Promise((resolve, reject) => {
    resolve('Promise resolved');  // Promise is resolved
    reject('Promise rejected');   // Promise can't be rejected
});
```

# Promise 的使用
我们通过调用 `then` 和 `catch` 方法来使用 promise。
语法为：
`.then()`: promise.then(successCallback, failureCallback)
成功的时候会调用 successCallback，失败的时候会调用 failureCallback。

举个例子：
```js
const promise = new Promise((resolve, reject) => {
    const randomNumber = Math.random();
    if (randomNumber < 0.7) {
        resolve('All things went well!');
    } else {
        reject(new Error('Something went wrong'));
    }
});
promise.then((data) => {
    console.log(data);  // prints 'All things went well!'
}, (error) => {
    console.log(error); // prints Error object
});
```

`.catch()`: promise.catch(failureCallback)
我们可以使用 `catch` 来捕获错误，它的可读性要强于前面使用的 `failureCallback`。
```js
const promise = new Promise((resolve, reject) => {
    reject(new Error('Something went wrong'));
});
promise
    .then((data) => {
        console.log(data);
    })
    .catch((error) => {
        console.log(error); // prints Error object
    });
```

# 链式调用
`then` 和 `catch` 函数会返回一个新的 promise 对象，所以它可以继续被捕获，从而形成链式调用。例如：
```js
const promise1 = new Promise((resolve, reject) => {
    resolve('Promise1 resolved');
});
const promise2 = new Promise((resolve, reject) => {
    resolve('Promise2 resolved');
});
const promise3 = new Promise((resolve, reject) => {
    reject('Promise3 rejected');
});
promise1
    .then((data) => {
        console.log(data);  // Promise1 resolved
        return promise2;
    })
    .then((data) => {
        console.log(data);  // Promise2 resolved
        return promise3;
    })
    .then((data) => {
        console.log(data);
    })
    .catch((error) => {
        console.log(error);  // Promise3 rejected
    });
```
解析上述代码：
- When promise1 is resolved, the `then()` method is called which returns promise2.
- The next `then()` is called when promise2 is resolved which returns promise3.
- Since promise3 is rejected, the next `then()` is not called instead `catch()` is called which handles the promise3 rejection.

## 常见错误
很多的新手会错误地嵌套 promise，这样做依然可以正常运行，但是代码不利于阅读。
```js
const promise1 = new Promise((resolve, reject) => {
    resolve('Promise1 resolved');
});
const promise2 = new Promise((resolve, reject) => {
    resolve('Promise2 resolved');
});
const promise3 = new Promise((resolve, reject) => {
    reject('Promise3 rejected');
});

promise1.then((data) => {
    console.log(data);  // Promise1 resolved
    promise2.then((data) => {
        console.log(data);  // Promise2 resolved

        promise3.then((data) => {
            console.log(data);
        }).catch((error) => {
            console.log(error);  // Promise3 rejected
        });
    }).catch((error) => {
        console.log(error);
    })
}).catch((error) => {
    console.log(error);
});
```

# Promise.all()
`Promise.all()` 可以将多个 Promise 实例包装成一个新的 Promise 实例。同时，成功和失败的返回值是不同的，成功的时候返回的是一个结果数组，而失败的时候则返回最先被 reject 变为失败状态的值，并且 reject 的是**第一个**抛出的错误信息。例如：
```js
const promise1 = new Promise((resolve, reject) => {
    setTimeout(() => {
        resolve('Promise1 resolved');
    }, 2000);
});
const promise2 = new Promise((resolve, reject) => {
    setTimeout(() => {
        resolve('Promise2 resolved');
    }, 1500);
});
Promise.all([promise1, promise2])
    .then((data) => console.log(data[0], data[1]))
    .catch((error) => console.log(error));
```

当您有多个 promise，并且希望知道所有 promise 何时可以 resolved，这个方法非常有用。例如，如果您正在从不同的 API 请求数据，并且仅当所有请求都成功时才希望对数据执行某些操作。

# Promise.race()
`Promise.race()` 方法返回一个 promise，一旦迭代器中的某个 promise 解决或拒绝，返回的 promise 就会解决或拒绝。例如：
```js
const promise1 = new Promise((resolve, reject) => {
    setTimeout(() => {
        resolve('Promise1 resolved');
    }, 1000);
});
const promise2 = new Promise((resolve, reject) => {
    setTimeout(() => {
        reject('Promise2 rejected');
    }, 1500);
});
Promise.race([promise1, promise2])
    .then((data) => console.log(data))  // Promise1 resolved
    .catch((error) => console.log(error));
```

# Promise.any()
这个方法用于返回第一个成功的 promise 。只要有一个 promise 成功此方法就会终止，它不会等待其他的 promise 全部完成。
`Promise.any()` 与 `Promise.race()` 的区别：
不像 `Promise.race()` 总是返回第一个结果值（resolved / reject）那样，这个方法返回的是第一个成功的值。这个方法将会忽略掉所有被拒绝的 promise，直到第一个 promise 成功。

# Promise.allSettled()
`Promise.allSettled()` 与 `Promise.all ()` 的区别：
- 它们所返回的数据不太一样，`Promise.all()` 返回一个直接包裹 resolve 内容的数组，则 `Promise.allSettled()` 返回一个包裹着对象的数组。
- 对于 `Promise.all()` 来说，如果有一个 Promise 对象报错了，则 `Promise.all()` 无法执行，会直接报错，无法获得其他成功的数据。而 `Promise.allSettled()` 方法是不管有没有报错，把所有的 Promise 实例的数据都返回回来，放入到一个对象中。如果是 resolve 的数据则 status 值为 fulfilled，否则为 rejected。

原文：
https://blog.bitsrc.io/understanding-promises-in-javascript-c5248de9ff8f
参考资料：
https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise
https://blog.csdn.net/iloveyu123/article/details/116588214