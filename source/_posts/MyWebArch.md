---
title: 写一个自己的Web前端框架
date: 2018-01-02 13:05:19
tags:
- web
- javascript
categories:
- web前端
---

"首先新年快乐！"


![](MyWebArch/NewYear.jpg)
### 前言
> 最近一直在看动脑学院的阿里前端p6架构师培训计划的直播，昨天一场比较精彩。讲的是一套架构，其称之为“驱动框架”。今天做一下总结。

### 一些技巧
- 后端返回的JSON数据键名与本地模块名相同（便于获取）
- JSON数据键名与HTML表单标签元素id相同（便于渲染）
> 为什么这样呢？在调用的时候非常方便，直接处理，可以看看以下代码。

### 框架

```javascript
var MetaDriver = (function (global,factory) {
    return factory.call(this);
})(this,function () {
    var _META_ = {
        init:function (data) {
            alert("init OK!");
            this.fetch(data)
            this.render()
        },
        fetch:function (data) {
            for (const m in _MODULE_) {
                if (_MODULE_.hasOwnProperty(m)) {
                    const element = _MODULE_[m];
                    _MODULE_[m].data = data[m];
                }
            }
        },
        render:function(){
            for(var m in _MODULE_){
                _MODULE_[m].render()
            }
        }
    };
    var _MODULE_ = {
        module1:{
            data:null,
            view:"hello",
            before:function () {
                console.log("before")
            },
            render:function () {
                if(this.before){
                    this.before();
                }
                alert(this.view);
                if(this.after){
                    this.after();
                }
            },
            after:function () {
                console.log("after")
            }
        }
    }
    return _META_;
})
```

### 个人理解
- 让自己的模块存在于一个闭包里，避免变量名冲突覆盖。jquery源码便有此种方式。
 ```javascript
let MyModuleDriver = (function(global,factory){
    return factory.call(global);
})(this,function(){
    let _META_ = {
        init:function(){}
    }
    return _META;
})
 ```
- 分治与统一管理

> 所谓的分治就是页面的不同模块区分管理，就是以上代码的__MODULE__部分。
每一个页面模块分一个module，每一个module都有自己管理的内容。包括**数据（data）、视图（view）、渲染器（render）**等等。这有点类似于现在主流的框架，比如Vuejs。<br>
所谓的统一管理是指驱动全部模块，就是以上代码__META__部分。有个初始化方法(init)，用于加载一些一次性的功能，然后有数据加载函数(fetch)用于将某些模块的数据进行赋值，也有整体渲染器，用于渲染所有模块。
### 更加神奇的一点
> 这一点直接听得我两眼放光，真所谓返璞归真、大道归一。
在防止数据重复加载方面炒鸡有意思。比如用相同的表单请求的是相同的数据，他要重新渲染视图。此时可以给模块加一个渲染锁。
```javascript
var _MODULE_ = {
        module1:{
            data:{},
            view:"hello",
            lock:false,
            before:function () {
                console.log("before")
            },
            render:function () {
                if(this.before){
                    this.before();
                }
                if(this.lock){
                    alert(this.view);
                    this.lock = false;
                }
                if(this.after){
                    this.after();
                }
            },
            after:function () {
                console.log("after")
            }
        }
    }
    //有新数据来的时候_META_开锁
    _META_={
        fetch:function (data) {
            for (const m in _MODULE_) {
                if (_MODULE_.hasOwnProperty(m)) {
                    const element = _MODULE_[m];
                    _MODULE_[m].data = data[m];
                    // 开锁！
                    _MODULE_[m].lock = true;
                }
            }
        }
    }
    
```
> 这里的lock起了关键的作用，这一点类似于同步锁，一样的道理。当有新数据来的时候才加载，没有新数据的时候锁一直是关着的，直接越过加载。

### 调用
- 在相应的HTML中引用此JS
- 直接调用MyMetaDriver(闭包返回的__META__)中的方法即可，类似调用即可。