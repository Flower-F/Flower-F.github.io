---
title: var、let、const辨析
date: 2021-12-11 19:26:49
tags: [javascript]
copyright: true
---
# var
1. 有变量提升
2. 没有块的概念，可以跨块访问；作用域只存在于函数中，不可以跨函数访问
3. 可以重复声明

# let
1. 有块级作用域
2. 没有变量提升，所以要声明后使用。（准确的说法是 let 的变量会进行初始化，但是不会像 var 的变量一样赋给一个 undefined 的值）
3. 存在暂时性死区。在块级作用域内，let 声明之前的部分被称为暂时性死区，暂时性死区内不可以使用 let 声明的变量
4. 不允许重复声明

# const
1. 具备 let 所具有的上述特性
2. 一旦声明必须立即赋值
3. 声明之后值不可以改变。（对于引用类型的变量而言，指的是它的地址不能发生改变）

# Object.freeze()
既然 const 对于引用类型只能确保地址不变，那么怎样才能使得声明的引用类型变量上的值无法改变呢？
这里我们可以使用 `Object.freeze()` 方法递归解决

```js
const myFreeze = obj => {
    Object.freeze(obj);
    Object.keys(obj).forEach(key => {
        if(typeof obj[key] === 'object') {
            myFreeze(obj[key]);
        }
    })
}
```