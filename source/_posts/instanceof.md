---
title: instanceof 详解
date: 2021-12-10 13:02:22
tags: [算法]
copyright: true
---
题目链接：
https://www.nowcoder.com/practice/a1169935fd6145899f953ba8fbccb585

# instanceof 用处

`instanceof` 判断对象的原型链上是否存在构造函数的原型。只能判断引用类型。

# instanceof 原理

instanceof 主要的实现原理：只要右边变量的 prototype 在左边变量的原型链上即可。因此， instanceof 在查找的过程中会遍历左边变量的原型链，直到找到右边变量的 prototype ，如果查找失败，则会返回 false

# instanceof 实现

过程：

1.  获取左边变量的隐式原型（即：`__ proto __` ,可通过 `Object.getPrototypeOf()` 获取）

2. 获取右边变量的显示原型（即：`prototype`）

3. 进行判断，比较 leftVal. __ proto __ . __ proto __ ……  === rightVal.prototype，相等则返回 true，否则返回 false

```js
function myInstanceof(left, right) {
    let leftProto = Object.getPrototypeOf(left); // 左边隐式原型
    const rightProto = right.prototype; // 右边显示原型
    
    // 在左边的原型链上查找
    while(leftProto !== null) {
        if(leftProto === rightProto) {
            return true;
        }
        leftProto = Object.getPrototypeOf(leftProto);
    }
    
    return false;
}
```

参考资料：公众号前端点线面