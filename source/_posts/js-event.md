---
title: js 事件机制
date: 2022-01-11 12:16:03
tags: [javascript]
copyright: true
---
# 事件捕获
当鼠标点击或者触发 DOM 事件时，浏览器会从根节点开始由外到内进行事件传播，即点击了子元素，如果父元素通过事件捕获方式注册了对应的事件的话，会先触发父元素绑定的事件

# 事件冒泡
与事件捕获恰恰相反，事件冒泡顺序是由内到外进行事件传播，直到根节点

![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/20220111124002.png)

# 示例
```html
<style>
    #div1 {
        display: flex;
        justify-content: center;
        align-items: center;
        width: 200px;
        height: 200px;
        background-color: red;
    }
    #div2 {
        width: 100px;
        height: 100px;
        background-color: green;
    }
</style>

<div id="div1">
  <div id="div2"></div>
</div>

<script>
    const div1 = document.getElementById('div1');
    const div2 = document.getElementById('div2');
    
    div1.onclick = function(){
        alert('1');
    }
    
    div2.onclick = function(){
        alert('2');
    }
</script>
```

当点击 div2 时，会弹出两个弹出框。在 Chrome 浏览器，会先弹出 2 再弹出 1，这就是事件冒泡：事件从最底层的节点向上冒泡传播。事件捕获则跟事件冒泡相反。

W3C 的标准是先捕获再冒泡， `addEventListener` 函数的第三个参数决定把事件注册在捕获（true）还是冒泡（false）。

# 事件流阻止
`event.preventDefault()`：取消事件对象的默认动作以及继续传播

# 事件委托
事件冒泡还允许我们利用**事件委托**这个概念依赖于这样一个事实，如果你想要在大量子元素中单击任何一个都可以运行一段代码，您可以将事件监听器设置在其父节点上，并让子节点上发生的事件冒泡到父节点上，而不是每个子节点单独设置事件监听器。

一个很好的例子是一系列列表项，如果你想让每个列表项被点击时弹出一条信息，您可以将click单击事件监听器设置在父元素 `<ul>` 上，这样事件就会从列表项冒泡到其父元素 `<ul>` 上。

例如，假设现在有 100 个 li，每个 li 有相同的点击事件。如果为每个 li 都添加事件，则会造成 DOM 访问次数过多，引起浏览器重绘与重排的次数过多，因此性能会降低。使用事件委托则可以解决这样的问题。

实现事件委托是利用了事件的冒泡原理实现的。当我们为最外层的节点添加点击事件，那么里面的 ul、li、a 的点击事件都会冒泡到最外层节点上，委托它代为执行事件。

```html
<ul id="ul">
    <li>1</li>
    <li>2</li>
    <li>3</li>
</ul>

<script>
    window.onload = function () {
        const ul = document.getElementById('ul');
        ul.onclick = function (ev) {
            const target = ev.target;
            if (target.nodeName.toLowerCase() === 'li') {
                alert(target.innerHTML);
            }
        }
    }
</script>
```

参考资料：
https://developer.mozilla.org/zh-CN/docs/Learn/JavaScript/Building_blocks/Events