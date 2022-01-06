---
title: 由 setTimeout 引发的连环追问
date: 2022-01-06 21:28:39
tags: [javascript]
copyright: true
---
# 基础问
```js
for (var i = 0; i < 5; i++) {
    setTimeout(function() {
        console.log(i);
    }, 1000);
}

console.log(i);
```
这段代码的执行结果是什么？
答案：立刻输出一个 5，1s 后再一次性输入 5 个 5。
解释：
- 因为 setTimeout 是异步函数，每一次 for 循环的时候，setTimeout 都执行一次，但是里面的函数没有被执行，而是被放到了任务队列里，等待执行。只有主线程上的任务执行完，才会执行任务队列里的任务。也就是说它会等到 for 循环全部运行完毕后，才会执行 setTimeout 里面的函数，但是当 for 循环结束后此时 i 的值已经变成了 5，因此控制台上的内容会输出 5 个 5。
- 关于这 5 个 5 为什么是一次性输出的：这里涉及到 JS 中的**定时器机制**，循环执行过程中，几乎同时设置了 5 个定时器。一般情况下，这些定时器都会在大约 1 秒之后触发，而循环完的输出是立即执行的，因此会立即输出 5 个 5。

# 改造的思路
如果期望代码的输出变成 1s 后输出 0，1，2，3，4，该怎么改造代码？
首先思考为什么会输出 5 个 5 呢？究其缘由，var 声明的变量不存在块级作用域，所以最终的 i 都是同一个 i。也就是说，i 是声明在全局作用域中的。所以如果我们可以使 i 声明在私有作用域中，就可以解决这个 bug。

# 闭包
因此，我们可以利用闭包，让 i 在每次迭代的时候，都产生一个私有的作用域，在这个私有的作用域中保存当前 i 的值。

```js
for (var i = 0; i < 5; i++) {
    (function(j) {  // 相当于 j = i
        setTimeout(function() {
            console.log(j);
        }, 1000);
    })(i);
}

console.log(i);
```

# let
接着我们很自然地会想到使用 let，let 本身当然是可以的，但是此处代码的最后一行十分恶心，要求输出 i，所以会报错。

# 拆分结构
闭包解决方法的最大问题就是可读性不好。
我们可以利用一个特性：JS 中基本类型的参数传递是**按值传递**的。
```js
function output(i) {
    setTimeout(function () {
        console.log(i);
    }, 1000);
}
for (var i = 0; i < 5; i++) {
    output(i); // 此处传过去的 i 被复制了
}

console.log(i);
```

# setTimeout 的第三个参数
```js
for (var i = 0; i < 5; i++) {
    setTimeout(function (j) {
        console.log(j);
    }, 1000, i);
}
```

# 如果想要每隔 1s 输出一次怎么办
```js
for (var i = 0; i < 5; i++) {
    (function (j) {
        setTimeout(function () {
            console.log(j);
        }, 1000 * j);
    })(i);
}

setTimeout(() => {
    console.log(i);
}, 1000 * i);
```

这算是一种解决方法，但是太粗暴了（x

# 使用 ES6 优化（Promise）
```js
const tasks = []; // 这里存放异步操作的 Promise
const output = (i) => new Promise((resolve) => {
    setTimeout(() => {
        console.log(i);
        resolve();
    }, 1000 * i);
});

// 生成全部的异步操作
for (var i = 0; i < 5; i++) {
    tasks.push(output(i));
}

// 进行异步操作；当异步操作完成之后，输出最后的 i = 5
Promise.all(tasks).then(() => {
    setTimeout(() => {
        console.log(i);
    }, 1000);
});
```

# 使用 ES7 优化（await）
使用 `await` 优化其实是很简单的，实际上就相当于写同步，只要实现一个 sleep 函数就行了。（bks 异步的最终解决方案
```js
// 模拟其他语言中的 sleep 函数
const sleep = (timeout) => new Promise((resolve) => {
    setTimeout(resolve, timeout);
});

(async () => {  // 声明立即执行的 async 函数表达式
    for (var i = 0; i < 5; i++) {
        if (i > 0) {
            await sleep(1000);
        }
        console.log(i);
    }

    // 输出最后的 5
    await sleep(1000);
    console.log(i);
})();
```

参考资料：
https://juejin.cn/post/6844903474212143117
https://juejin.cn/post/6844903612879994887
https://juejin.cn/post/6844903841888993287