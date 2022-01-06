---
title: 最长公共前缀
date: 2021-12-08 21:01:53
tags: [算法, codetop]
copyright: true
---
题目链接：
https://leetcode-cn.com/problems/longest-common-prefix/
解法分析：让数组中的第一个元素成为前缀，然后循环字符串数组，判断数组中的每一个元素是否与声明的那个前缀一样，不一样就将声明的那个前缀长度减一，循环直到找到每一个元素都开头的那个前缀。用到了字符串 startswith 方法和 substring 方法，非常巧妙。

```js
/**
 * @param {string[]} strs
 * @return {string}
 */
var longestCommonPrefix = function(strs) {
    // 让数组中的第一个元素成为前缀
    let prefix = strs[0];
    // 循环字符串数组，判断数组中的每一个元素是否与声明的前缀一样
    for(let i = 1; i < strs.length; i++) {
        while(!strs[i].startsWith(prefix)) {
            // 不一样就将声明的前缀长度减一
            prefix = prefix.substring(0, prefix.length - 1);
            // 判断是否存在公共前缀
            if(prefix === "") {
                return "";
            }
        }
    }
    return prefix;
};
```

解法参考：
作者：gallant-dijkstra
链接：
https://leetcode-cn.com/problems/longest-common-prefix/solution/jian-dan-ti-zhong-quan-chu-ji-by-gallant-t85e/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。