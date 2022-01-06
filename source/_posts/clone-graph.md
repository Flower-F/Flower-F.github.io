---
title: 克隆图
date: 2021-12-16 17:49:48
tags: [算法, leetcode]
copyright: true
---
题目链接：
https://leetcode-cn.com/problems/clone-graph/
解法分析：深度优先遍历，递归新建每个节点和相邻节点的关系。

```ts
/**
 * Definition for Node.
 * class Node {
 *     val: number
 *     neighbors: Node[]
 *     constructor(val?: number, neighbors?: Node[]) {
 *         this.val = (val===undefined ? 0 : val)
 *         this.neighbors = (neighbors===undefined ? [] : neighbors)
 *     }
 * }
 */


function cloneGraph(node: Node | null): Node | null {
    if (node === null) {
        return null;
    }
    const visited = new Map<Node, Node>();
    dfs(node);
    return visited.get(node) as Node | null; // 返回根节点对应的克隆节点

    function dfs(node: Node): void {
        // 拷贝节点
        const copyNode = new Node(node.val);
        // 节点值和新建节点以键值对形式存入 visited
        visited.set(node, copyNode);
        // 遍历当前节点的所有相邻节点
        (node.neighbors || []).forEach(neighbor => {
            if (!visited.has(neighbor)) {
                // 递归遍历相邻节点
                dfs(neighbor);
            }
            // 如果该节点已经被访问过了，则直接从哈希表中取出对应的克隆节点返回
            copyNode.neighbors.push(visited.get(neighbor));
        })
    }
};
```

参考题解：
作者：chen-wei-f
链接：
https://leetcode-cn.com/problems/clone-graph/solution/133-ke-long-tu-by-chen-wei-f-2chn/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。