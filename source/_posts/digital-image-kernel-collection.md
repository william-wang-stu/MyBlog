---
title: DIP中常见的核
mathjax: true
copyright: true
comment: true
no-emoji: false
date: 2019-03-14 19:01:20
tags:
- Computer Graphics
- DIP
categories: 
- Computer Science
- Computer Graphics
photo: https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1552632955568&di=95596c734f92216417197cc0f4616e69&imgtype=0&src=http%3A%2F%2Fgetwallpapers.com%2Fwallpaper%2Ffull%2F4%2F8%2Fd%2F371056.jpg
---

{% note default %}
一二三四五，六七八九十。
{% endnote %}

<!-- more -->

---

{% note default %}

**文章暂未完成**

{% endnote %}


## 前言

DIP（Digital Image Processing）中的核可以看做是一个模具，可以作用于每个像素。每种核作用图像后会得到不同的结果，本文主要是记录一些常见的“核”

视频见[YOUTOBE](https://www.youtube.com/watch?v=izDu5zdjjYA)

{% note info%}
## 核是什么

核可以看做是一个模具，也叫做模板，有时也称算子。模板操作是数字图像处理中常用的一种邻域运算方式，主要有卷积和相关两种，可以实现图像平滑、图像锐化、边缘检测等功能。

- 核常用矩阵表示。可以是一幅图像、一个滤波器或一个窗口，定义了参与运算的中心元素和邻域元素的相对位置及相关系数。卷积时常采用3×3或者5×5大小。
- 模板的中心元素（或称原点）表示将要处理的元素， 一般取模板中心点，也可根据需要选取非中心点。

{% endnote %}

## 常见的核

### 图像平滑

#### 邻域平均滤波
> 邻域平均法是一种线性低通滤波器。**可滤除一定的噪声。**

- 第一种

$$
    \frac{1}{9}\times
    \begin{bmatrix}
        1&1&1\\\\
        1&1&1\\\\
        1&1&1
    \end{bmatrix}
$$

- 第二种

$$
    \frac{1}{16}\times
    \begin{bmatrix}
       1&2&1\\\\
       2&4&2\\\\
       1&2&1 
    \end{bmatrix}
$$

#### 中值滤波

> 是一种非线性滤波，**找出核内的中位数当做结果**。

$$
       Midian( 
           \begin{bmatrix}
             a&b&c\\\\
             d&e&f\\\\
             g&h&i 
           \end{bmatrix})
$$

### 图像锐化

