---
title: new 详解
date: 2022-01-11 12:07:09
tags: [javascript]
copyright: true
---
# new 关键字做了什么
- 创建一个新对象
- 将新对象原型设置为构造函数原型
- 使用 apply 绑定 this
- 返回结果，需要特判上一步的返回值是否是 Object 类型

# 手写 new
```js
function myNew(fn, ...args) {
    // 创建一个新的对象
    const obj = {}
    // 把该对象的原型设置为 fn 的原型
    if (fn.prototype !== null) {
        Object.setPrototypeOf(obj, fn.prototype);
    }
    // 使用 apply，改变构造函数 this 的指向到新建的对象
    // 这样 obj 就可以访问到构造函数中的属性
    const result = fn.apply(obj, args);

    return result instanceof Object ? result : obj;
}

function Person(name, age) {
    this.name = name;
    this.age = age;
}

// const person = new Person('Jack', 12);
const person = myNew(Person, 'Jack', 12);
console.log(person);
```