---
title: 亲密字符串
date: 2021-12-13 14:21:54
tags: [算法, codetop]
copyright: true
---
题目链接：
https://leetcode-cn.com/problems/buddy-strings/
解法分析：这题真的是细节非常多啊，其中很容易忽略的一个细节就是，当两个字符串完全相同，且 s 字符串中存在两个相同的字符串时，它们也是亲密字符串。所以这种情况还需要特判一下。

```ts
function buddyStrings(s: string, goal: string): boolean {
    if (s.length < 2 || s.length !== goal.length) {
        return false;
    }
    const ans = [];
    for (let i = 0; i < s.length; i++) {
        if (s[i] !== goal[i]) {
            ans.push(s[i]);
            ans.push(goal[i]);
        }
        if (ans.length > 4) {
            return false;
        }
    }

    // 特判两字符串相等的情况
    if(ans.length === 0) {
        const set = new Set<string>();
        for(let i = 0; i < s.length; i++) {
            if(set.has(s[i])) {
                return true;
            } else {
                set.add(s[i]);
            }
        }
        return false;
    }

    if (ans.length !== 4) {
        return false;
    }

    return ans[0] === ans[3] && ans[1] === ans[2];
};
```