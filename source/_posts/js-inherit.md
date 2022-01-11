---
title: js 继承
date: 2022-01-11 12:03:19
tags: [javascript]
copyright: true
---
常见的继承主要有三种：
- 组合继承
- 寄生组合继承
- ES6 的 class

# 组合继承
- 子类的构造函数中通过 `Parent.call(this)` 继承父类的属性
- 改变子类的原型为 `new Parent()` 来继承父类的函数

```js
function Parent(val) {
    this.val = val;
}
Parent.prototype.getValue() = function() {
    return this.value;
}

function Child(val) {
    Pareng.call(this, val);
}
Child.prototype = new Parent();

const child = new Child(1);
console.log(child);
console.log(child instanceof Parent);
```

缺点：
- 使用组合继承时，父类构造函数会被调用两次
- 生成了两个实例，但是子类实例中的属性和方法会覆盖子类原型(父类实例)上的属性和方法，所以造成了子类的原型中多了一些不必要的属性，增加了不必要的内存

# 寄生组合继承
```js
// 只要修改一行代码即可
// Child.prototype = new Parent();
Child.proptotype = Object.create(Parent.prototype);
```

# class 继承
```js
class Parent {
    constructor(val) {
        this.val = val;
    }
    getValue() {
        return this.val;
    }
}

class Child extends Parent {
    constructor(val) {
        super(val);
    }
}

const child = new Child(1);
console.log(child.getValue());
console.log(child instanceof Parent);
```
