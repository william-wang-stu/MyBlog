---
title: BMP文件的特殊宽度处理及字节对齐问题
mathjax: true
copyright: true
comment: true
date: 2019-03-08 08:10:54
tags:
- Computer Graphics
- DIP
categories: Computer Graphics
photo: http://www.33lc.com/article/UploadPic/2012-10/2012101811361423324.jpg
---

{% note default %}
所见所观，皆为虚幻。
{% endnote %}

<!-- more -->

---

## 前言

[BMP文件处理（C语言实现）](/2019/03/06/bitmap-resolver/)这篇文章中说到了两个不足之处：

- 文件头的字节对齐问题
- BMP文件的特殊宽度处理问题

对于字节对齐，可以查看[计算机的字节对齐](/2019/03/08/byte-alignment/)这篇文章。本篇文章主要写对BMP文件的特殊宽度处理。



## 文件头的字节对齐问题


## BMP文件的特殊宽度处理

[BMP文件处理（C语言实现）](/2019/03/06/bitmap-resolver/)这篇文章中说到BMP的宽度字节数规定必须为4的倍数，文章中的代码读取BMP宽度字节数为非4倍数并且不为3的倍数时（是3倍但不是4倍时，不会造成像素偏移），会造成像素偏移问题，效果如下：

> 大图没有保留，只剩小的测试图了，不过效果就是有线条（）


## 总结