---
title: 字节前端青训营第 6 天（上）
date: 2022-01-21 10:02:29
tags: [webgl]
copyright: true
---
这部分的入门门槛太高了，等我先把基础学好了再研究吧。

# 现代图形系统

- 光栅 Raster：几乎所有的现代图形系统都是基于光栅来绘制图形的，光栅就是指构成图像的像素阵列。
- 像素 Pixel：一个像素对应图像上的一个点，它通常保存图像上的某个具体位置的颜色等信息。
- 帧缓存 Frame Buffer：在绘图过程中，像素信息被存放于帧缓存中，帧缓存是一块内存地址。
- CPU (Central Processing Unit)：中央处理单元，负责逻辑计算。
- GPU (Graphics Processing Unit)：图形处理单元，负责图形计算。

# CPU VS GPU


# WebGL Startup

1. 创建 WebGL 上下文（Canvas）
2. 创建 WebGL Program（顶点选择器、片源选择器）
3. 将数据存入缓冲区（比如顶点颜色）
4. 将缓冲区数据读取到 GPU
5. GPU 执行 WebGL 程序，输出结果
