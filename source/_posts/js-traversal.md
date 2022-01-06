---
title: js 遍历对象
date: 2021-12-13 00:21:49
tags: [javascript]
copyright: true
---
本文总结 js 遍历对象的方法。
下面是本文所用的测试对象：
```js
const parentObj = {
    a: 'aaaaa',
    b: Symbol('bbbbb'),
    c: 'ccccc'
};
const obj = Object.create(parentObj, {
    d: {
        value: 'ddddd',
        enumerable: true
    },
    e: {
        value: 'eeeee',
        enumerable: false
    },
    [Symbol('f')]: {
        value: 'fffff',
        enumerable: true
    }
});
```

# 9 种遍历方法
## Object.keys(obj)
Object.keys 返回一个所有元素为字符串的数组，其元素来自于从给定的 object 上面**可直接枚举**的属性(**不含 Symbol 属性**）。
```js
console.log('Object.keys(): ', Object.keys(obj)); 
// Object.keys():  [ 'd' ]
```

## Object.values(obj)
Object.values() 返回一个数组，其元素是在对象上找到的**可枚举**属性值。
```js
console.log('Object.values():', Object.values(obj)); 
// Object.values(): [ 'ddddd' ]
```

## Object.entries(obj)
Object.entries() 返回一个数组，其元素是与直接在 object 上找到的**可枚举属性**键值对相对应的数组。
```js
console.log('Object.entries():', Object.entries(obj)); 
// Object.entries(): [ [ 'd', 'ddddd' ] ]
```

## Object.getOwnPropertyNames(obj)
Object.getOwnPropertyNames() 方法返回一个由指定对象的所有自身属性的属性名（**包括不可枚举属性但不包括 Symbol 值**作为名称的属性）组成的数组。
```js
console.log('Object.getOwnPropertyNames():', Object.getOwnPropertyNames(obj)); 
// Object.getOwnPropertyNames(): [ 'd', 'e' ]
```

## Object.getOwnPropertySymbols(obj)
Object.getOwnPropertySymbols() 方法返回一个给定对象自身的所有 Symbol 属性的数组。
```js
console.log('Object.getOwnPropertySymbols()', Object.getOwnPropertySymbols(obj));
// Object.getOwnPropertySymbols() [ Symbol(f) ]
```

##  for...in...
遍历所有**可枚举**的属性（包括原型上的），然后可利用 hasOwnProperty 判断对象是否包含特定的自身（非继承）属性。
注意事项：
1. index 索引为字符串型数字，不能直接进行几何运算
2. 遍历顺序有可能不是按照实际数组的内部顺序
3. 会遍历数组的所有可枚举属性，包括原型
4. for...in 更适合便利对象，不要使用 for...in 遍历数组

```js
for (let key in obj) {
    console.log('for in:', key);
}
// for in: d
// for in: a
// for in: b
// for in: c
```
## for...of...
必须部署了 **Iterator** 接口后才能使用。使用范围：数组、Set 和 Map 结构、类数组对象（arguments、DOMNodeList对象……）、Generator 对象以及字符串。
```js
for (let value of Object.values(obj)) {
    console.log('for of:', value);
}
// for of: ddddd
```

## forEach
使用 break 不能中断循环使用。
```js
Object.values(obj).forEach(value => {
    console.log('forEach:', value);
});
// forEach: ddddd
```

## Reflect.ownKeys(obj)
返回一个数组，包含对象自身的**所有属性**，不管属性名是 Symbol 还是字符串，也不管是否可枚举。
```js
console.log('Reflect.ownKeys():', Reflect.ownKeys(obj));
// Reflect.ownKeys(): [ 'd', 'e', Symbol(f) ]
```

# 特点总结
![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/123131212.JPG)

# 遍历顺序
上述遍历对象的属性时都遵循同样的属性遍历次序规则：
1. 首先遍历所有属性名为**数值**的属性，按照数字排序
2. 其次遍历所有属性名为**字符串**的属性，按照生成时间排序
3. 最后遍历所有属性名为 **Symbol** 值的属性，按照生成时间排序

参考资料：公众号前端点线面