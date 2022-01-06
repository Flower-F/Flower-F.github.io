---
title: LRU 缓存机制
date: 2021-12-24 09:28:55
tags: [算法, leetcode]
copyright: true
---
题目链接：
https://leetcode-cn.com/problems/lru-cache/
解法分析：LRU 经典模拟题，使用双链表 + 哈希组合的方法解决。

```js
/** 
 * @param {number} key
 * @param {number} value
 */
var DoubleLinkedListNode = function(key, value) {
    this.key = key;
    this.value = value;
    this.prev = null;
    this.next = null;
}

/**
 * @param {number} capacity
 */
var LRUCache = function(capacity) {
    this.capacity = capacity;
    this.map = new Map();
    // 通过 dummy 节点减少边界处理
    this.dummyHead = new DoubleLinkedListNode(null, null);
    this.dummyTail = new DoubleLinkedListNode(null, null);
    this.dummyHead.next = this.dummyTail;
    this.dummyTail.prev = this.dummyHead;
};

/** 
 * @return {boolean}
 */
LRUCache.prototype._isFull = function() {
    return this.map.size === this.capacity;
}

/** 
 * @param {DoubleLinkedListNode}
 * @return {DoubleLinkedListNode}
 */
LRUCache.prototype._removeNode = function(node) {
    node.prev.next = node.next;
    node.next.prev = node.prev;
    // 删除节点
    node.prev = null;
    node.next = null;
    return node;
}

/** 
 * @param {DoubleLinkedListNode}
 */
LRUCache.prototype._addToHead = function(node) {
    const head = this.dummyHead.next;
    node.next = head;
    head.prev = node;
    node.prev = this.dummyHead;
    this.dummyHead.next = node;
}

/** 
 * @param {number} key
 * @return {number}
 */
LRUCache.prototype.get = function(key) {
    if (this.map.has(key)) {
        // get 的要放到链表头
        const node = this.map.get(key);
        this._addToHead(this._removeNode(node));
        return node.value;
    } else {
        return -1;
    }
};

/** 
 * @param {number} key 
 * @param {number} value
 * @return {void}
 */
LRUCache.prototype.put = function(key, value) {
    if (this.map.has(key)) {
        // 若已存在，修改值，然后将其放到链表头
        const node = this.map.get(key);
        node.value = value;
        this._addToHead(this._removeNode(node));
    } else {
        if (this._isFull()) {
            // 容量已满，去除尾节点，同时删除哈希表中的对应 key
            const node = this.dummyTail.prev;
            this.map.delete(node.key);
            this._removeNode(node);
        }
        // 添加头节点
        const node = new DoubleLinkedListNode(key, value);
        this.map.set(key, node);
        this._addToHead(node);
    }
};

/**
 * Your LRUCache object will be instantiated and called as such:
 * var obj = new LRUCache(capacity)
 * var param_1 = obj.get(key)
 * obj.put(key,value)
 */
```