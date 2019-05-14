---
title: 边缘检测
mathjax: false
copyright: true
comment: true
date: 2019-05-14 10:28:14
tags:
- Computer Graphics
- DIP
categories:
- Computer Science
- Computer Graphics
photo: https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1557811205606&di=0b7f51375eeec879cfca736ad1be8517&imgtype=0&src=http%3A%2F%2Fi0.hdslb.com%2Fbfs%2Farticle%2F210653cddb5404d31b8aa34d524447df75de3b36.jpg
---

{% note primary %}
变化的变化率，用于描述变化的变化速度和方向。
{% endnote %}

<!-- more -->

---

## Forword

很长时间不写博客了，图像处理实验开始整体验收了，这两天爆肝，在做实验的同时认真写博客总结。确实感觉这门课学得不好（实际上感觉大学所有的课都学的不好，逃……），主要是因为数学知识不够牢固，而且一般的数学证明思维也忘得差不多了。


本系列文章（DIP，Digital Image Processing），用于记录总结数字图像处理课程的实验内容。目录请参考[DIP系列文章](/tags/DIP/)。

{% note warning no-icon %}

**本次不展示代码，全部代码请查看[GitHub](https://github.com/ScarboroughCoral/DIPModule)**

{% endnote %}

## Body

这次实验内容是边缘检测，一开始的时候做了一部分了，是针对`prewitt`算子和`sobel`算子的，剩下`log`和`canny`方法还没做，canny选做。

### 边缘检测

