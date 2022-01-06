---
title: 删除字符串中的所有相邻重复项
date: 2021-12-11 18:22:36
tags: [算法, codetop]
copyright: true
---
题目链接：
https://leetcode-cn.com/problems/remove-all-adjacent-duplicates-in-string/
解法分析：消除一对相邻重复项可能会导致新的相邻重复项出现，如从字符串 abba 中删除 bb 会导致出现新的相邻重复项 aa 出现。因此我们需要保存当前还未被删除的字符。所以这里很适合使用数据结构栈。我们只需要遍历该字符串，如果当前字符和栈顶字符相同，我们就贪心地将其消去，否则就将其入栈即可。

```js
/**
 * @param {string} s
 * @return {string}
 */
var removeDuplicates = function(s) {
    const stack = [];
    
    for(const ch of s) {
        if(s.length > 0 && stack[stack.length - 1] === ch) {
            stack.pop();
        } else {
            stack.push(ch);
        }
    }
    
    return stack.join('');
};
```

参考题解：
作者：LeetCode-Solution
链接：
https://leetcode-cn.com/problems/remove-all-adjacent-duplicates-in-string/solution/shan-chu-zi-fu-chuan-zhong-de-suo-you-xi-4ohr/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。