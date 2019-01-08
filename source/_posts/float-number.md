---
title: 关于计算机浮点数的信息表示和机器级表示总结
date: 2018-12-25 15:51:44
img: https://thumbs.dreamstime.com/b/code-binaire-sur-l-%C3%A9cran-bleu-de-pixel-43712570.jpg
mathjax: true
tags:
- Computer Science
- CSAPP

categories: Computer Science
---


## Merry Xmas!

![](https://dn-linuxcn.qbox.me/data/attachment/album/201812/25/011455hp3tz2ydj3hjwtat.jpg)


## 前言


浮点数这一块一直是没怎么仔细看，从计算机导论到数字逻辑再到机组，一直是划水，借这个机会在此总结一下，若有偏颇之处，请在下方评论区指正。

## 浮点数的信息表示


> 在计算机科学中，浮点（英语：floating point，缩写为FP）是一种对于实数的近似值数值表现法，由一个有效数字（即尾数）加上幂数来表示，通常是乘以某个基数的整数次指数得到。以这种表示法表示的数值，称为浮点数（floating-point number）。利用浮点进行运算，称为浮点计算，这种运算通常伴随着因为无法精确表示而进行的近似或舍入。

### 如何实现信息表示

计算机只认识0和1。浮点表示对形如 $V = x \times 2^{y}$的有理数进行编码。它对执行涉及非常大的数字（$\vert V \vert \gg 0$）、非常接近于0（$\vert V \vert \ll 1$）的数字，以及更普遍的作为实数运算的近似值的计算，是很有用的。


针对形如$b_mb_{m-1}···b_0b_1.b_{-1}b_{-2}···b_{-n-1}b_{-n}$的二进制小数串可定义为$b=\sum_{i=-n}^{m}2^i\times b_i$

IEEE 浮点标准使用 $V = (-1)^s\times M\times 2^E$ 的形式来表示一个数：

- 符号（sign）s 决定这个数是负数（s=1）还是正数（s=0），对于数值 0 的 符号位解释作为特殊情况处理
- 尾数（significand） M 是一个二进制小数，它的范围是 $1\sim 2-\epsilon$
- 阶码（exponent）E 的作用是对浮点数加权，这个权重是2的E次幂（可能是负数）

## 浮点数的机器级表示

> 暂未完成


## 参考资料

### 引用作品

### 引用文献

- wikipedia.(2012).[浮点数](https://zh.wikipedia.org/wiki/%E6%B5%AE%E7%82%B9%E6%95%B0)


### 参阅书目

- Bryant.(2016).CSAPP