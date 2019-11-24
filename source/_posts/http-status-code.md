---
title: 协议系列：HTTP常见请求方式和状态码总结【WIP】
mathjax: false
copyright: true
comment: true
date: 2019-11-20 15:37:45
tags:
- Protocol
- HTTP
categories:
- Protocol
- HTTP
photo: http://pic1.win4000.com/wallpaper/2018-11-06/5be155d25c184.jpg
---

{% note primary %}
404 not found。
{% endnote %}

<!-- more -->

---

## 前言

最近在看[同源策略和跨域问题](/2019/11/19/secure-same-origin-policy/)时，看到CORS的解决方案是需要prefight，跨域时先向目标源发送一个OPTIONS请求，就发现这部分知识没有系统的了解过。今天就来总结一下常见的HTTP请求方式和HTTP状态码。

- [前言](#%e5%89%8d%e8%a8%80)
- [常见HTTP请求方式](#%e5%b8%b8%e8%a7%81http%e8%af%b7%e6%b1%82%e6%96%b9%e5%bc%8f)
- [常见HTTP状态码](#%e5%b8%b8%e8%a7%81http%e7%8a%b6%e6%80%81%e7%a0%81)
- [Reference](#reference)


## 常见HTTP请求方式

这些请求方式是case-sensitive的，但是一些请求工具或请求框架可能已经做了toUpperCase处理。

- GET（HTTP1.0）
  这个是最常见的请求方式了，
- POST（HTTP1.0）
- OPTIONS（HTTP1.1）
- HEAD（HTTP1.0）
- PUT（HTTP1.1）
- DELETE（HTTP1.1）
- TRACE（HTTP1.1）
- CONNECT（HTTP1.1）
- PATCH

## 常见HTTP状态码


## Reference

- [List of HTTP status codes](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes)
- [Hypertext Transfer Protocol](https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol)