---
title: 手写 Promise
date: 2022-01-11 16:24:37
tags: [javascript, 手写]
copyright: true
---
# Promise

```js
const PENDING = 'pending';
const FULFILLED = 'fulfilled';
const REJECTED = 'rejected';

class Promise {
    constructor(executor) {
        this.state = PENDING;
        this.value = null;
        this.reason = null;
        this.onResolvedCallbacks = [];
        this.onRejectedCallbacks = [];

        const resolve = (value) => {
            // 避免多次 resolve 或 reject
            if (this.state === PENDING) {
                this.state = FULFILLED;
                this.value = value;
                // 一旦 resolve 执行，调用成功数组的函数
                this.onResolvedCallbacks.forEach(fn => fn());
            }
        }

        const reject = (reason) => {
            if (this.state === PENDING) {
                this.state = REJECTED;
                this.reason = reason;
                // 一旦 reject 执行，调用失败数组的函数
                this.onRejectedCallbacks.forEach(fn => fn());
            }
        }

        try {
            // executor 同步执行，用户可以选择调用 resolve 和 reject
            executor(resolve, reject);
        } catch (error) {
            reject(error);
        }
    }

    then(onFulfilled, onRejected) {
        if (typeof onFulfilled !== 'function') {
            // 当不是函数时，onFulfilled 返回一个普通的值，成功时直接等于 value
            onFulfilled = value => value;
        }
        if (typeof onRejected !== 'function') {
            // 当不是函数时，onRejected 直接抛出错误
            onRejected = reason => {
                throw new Error(reason instanceof Error ? reason.message : reason);
            }
        }

        // onFulfilled 和 onRejected 都必须异步调用，而且严格意义上应该是微任务
        // 此处只是使用 setTimeout 来模拟异步

        // 若 onFulfilled 或 onRejected 报错，则直接 reject
        const promise2 = new Promise((resolve, reject) => {
            if (this.state === FULFILLED) {
                setTimeout(() => {
                    try {
                        const x = onFulfilled(this.value);
                        // resolvePromise 函数，处理自己 return 的 promise 和默认的 promise2 的关系
                        this.resolvePromise(promise2, x, resolve, reject);
                    } catch (error) {
                        reject(error);
                    }
                }, 0);
            }
            if (this.state === REJECTED) {
                setTimeout(() => {
                    try {
                        const x = onRejected(this.reason);
                        this.resolvePromise(promise2, x, resolve, reject);
                    } catch (error) {
                        reject(error);
                    }
                }, 0);
            }
            if (this.state === PENDING) {
                this.onResolvedCallbacks.push(() => {
                    setTimeout(() => {
                        try {
                            const x = onFulfilled(this.value);
                            this.resolvePromise(promise2, x, resolve, reject);
                        } catch (error) {
                            reject(error);
                        }
                    }, 0);
                })
                this.onRejectedCallbacks.push(() => {
                    setTimeout(() => {
                        try {
                            const x = onRejected(this.reason);
                            this.resolvePromise(promise2, x, resolve, reject);
                        } catch (error) {
                            reject(error);
                        }
                    }, 0);
                });
            }
        })

        return promise2;
    }

    // 处理不同的 promise 套用
    resolvePromise(promise2, x, resolve, reject) {
        if (promise2 === x) {
            return reject('Chaining cycle detected for promise')
        }

        let called = false; // 成功和失败只能调用一个，此处防止多次调用
        if (x instanceof Object) {
            try {
                const then = x.then;
                if (typeof then === 'function') {
                    then.call(x, y => {
                        if (called) return;
                        called = true;
                        // resolve 的结果依旧是 promise 那就继续解析
                        this.resolvePromise(promise2, y, resolve, reject);
                    }, error => {
                        if (called) return;
                        called = true;
                        reject(error);
                    })
                } else {
                    resolve(x);
                }
            } catch (error) {
                if (called) return;
                called = true;
                reject(error);
            }
        } else {
            resolve(x);
        }
    }
};
```

完成书写以后，可以使用 npm 包 `promises-aplus-tests` 进行测试。

测试前需要加上这几行代码：
```js
Promise.defer = Promise.deferred = function () {
    let dfd = {}
    dfd.promise = new Promise((resolve,reject)=>{
        dfd.resolve = resolve;
        dfd.reject = reject;
    });
    return dfd;
}
module.exports = Promise;
```

# Promise.resolve
`Promise.resolve` 会将任何值转成值为 value 状态是 fulfilled 的 Promise，但如果传入的值本身是 Promise 则会原样返回它。

```js
Promise.resolve = function(value) {
    if (value instanceof Promise) {
        return value;
    } 
    return new Promise(resolve => resolve(value));
};
```

# Promise.reject
和 `Promise.resolve` 类似，`Promise.reject` 会实例化一个 rejected 状态的 Promise。但与 `Promise.resolve` 不同的是，如果给 `Promise.reject` 传递一个 Promise 对象，则这个对象会成为新 Promise 的值。
```js
Promise.reject = function(reason) {
    return new Promise((resolve, reject) => reject(reason));
};
```

# Promise.all
```js
Promise.all = function(promiseArr) {
    let resolveCount = 0;
    const result = [];
    return new Promise((resolve, reject) => {
        promiseArr.forEach((promise, index) => {
            Promise.resolve(promise).then(val => {
                resolveCount++;
                result[index] = val;
                if (resolveCount === promiseArr.length) {
                    resolve(result);
                }
            }, error => {
                reject(error);
            })
        })
    })
};
```

# Promise.allSettled
```js
Promise.allSettled = function(promiseArr) {
    const result = [];

    return new Promise((resolve, reject) => {
        promiseArr.forEach((promise) => {
            Promise.resolve(promise).then(val => {
                result.push({
                    state: FULFILLED,
                    value: val
                })
            }, err => {
                result.push({
                    state: REJECTED,
                    value: err
                })
            })
            if (result.length === promiseArr.length) {
                resolve(result);
            }
        })
    })
    
}
```

# Promise.race
```js
Promise.race = function(promiseArr) {
    return new Promise((resolve, reject) => {
        promiseArr.forEach(promise => {
            Promise.resolve(promise).then(val => {
                resolve(val);
            }, err => {
                reject(err);
            })
        })
    })
}
```

# Promise.any
```js
Promise.any = function(promiseArr) {
    let rejectCount = 0;
    return new Promise((resolve, reject) => {
        promiseArr.forEach((promise) => {
            Promise.resolve(promise).then(val => {
                resolve(val);
            }, err => {
                rejectCount++;
                if (rejectCount === promiseArr.length) {
                    throw new Error('All promises were rejected')
                }
            })
        })
    })
}
```

参考资料：
https://juejin.cn/post/6844903625769091079
https://juejin.cn/post/6946022649768181774