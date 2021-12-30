---
title: 盒子模型
date: 2021-12-30 15:44:17
tags: [css]
copyright: true
---
# 盒子模型的分类
有 3 种盒子模型：IE 盒模型（border-box）、标准盒模型（content-box）、padding-box
盒模型一般都由四部分组成：内容（content）、填充（padding）、边界（margin）、边框（border）

# 盒子模型之间的区别
- 标准盒模型：width，height 只包含内容 content，不包含 border 和 padding
- IE 盒模型：width，height 包含 content、border 和 padding
- padding-box：width，height 包含 content、padding，不包含 border