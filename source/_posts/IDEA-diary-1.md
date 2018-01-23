---
title: IDEA 踩坑日记（一）
date: 2017-12-25 21:27:10
tags:
- Java Web
- IDEA
categories:
- Java Web
- IDEA 踩坑日记
---

### 找不到类
> 今天遇到了这样的情况，SE大作业的控制层，用了下servlet。现在的情况是前端能给后端发送数据并且后端能够接受了，后端却无法回应前端，一般想的是通过Servlet的doPost/doGet方法用Respone对象的PrintWriter将JSON传送回去，然后找JSON解析的类，找不到。然后又将普通的Web工程转成了Maven工程，pom.xml文件里加入了org.json的相应依赖，然后向前端返回json之后居然报错。这就是问题所在了。先来看看报的什么错：

```java
java.lang.NoClassDefFoundError: net/sf/json/JSONObject
```

> 很迷！咋回事，明明jar包已经到手，为什么还要报未找到class呢！


- 首先，我在项目结构中找到了以下配置。




![](IDEA-diary-1/idea-diary-1.png)

> 没问题啊，有class啊，为啥报错！

- 然后请教了学长，被他嘲讽了一番。然后是这儿。。原来这儿只是向项目中中添加了jar包，但前端请求时需要的是打包后的文件给它传送json数据。所以就是这儿了需要把相应的jar包放到打包目录下。

![](IDEA-diary-1/idea-diary-2.png)

> 需要将项目中已有的jar包（右方应该是编译时起作用的），但要想让网页请求成功，需要的是打包后的class文件，也就是out文件夹下。需要将已有的jar包放到此文件夹下。

![](IDEA-diary-1/idea-diary-3.png)

> 然后请求OK！