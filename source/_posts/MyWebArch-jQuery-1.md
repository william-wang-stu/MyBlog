---
title: 前端架构之jQuery源码借鉴
date: 2018-01-04 22:41:49
tags:
- web
- javascript
- jquery
categories:
- web前端
---

### “焦虑，不存在的”<br>

![](MyWebArch-jQuery-1/bz-bh-gewei.jpg)

### 说在前面的话
> 最近让我感觉用处最大的首先就是闭包了，没有变量污染，jQuery源码中整体便处于一个闭包之中，看来的确很有用。<br>
>另外，对于jquery，一直认为很便捷，非常喜欢使用这东西，有时候甚至忘了原生如何实现。比如$、$()这些到底是个什么？$().css()和$.ajax有什么管理区别。

--- 

### 代码&解析

#### $是什么？


```javascript
(function(global){

    var jQuery = function(){

    }
    global.$ = global.jQuery = jQuery;

})(this)

```
> 哦，$实际上就是window下的一个全局变量（加载过jquery之后），而且还是一个类似构造函数的东西。

#### $()是怎样实现的？

```javascript
    var jQuery = function(){
        return new jQuery();
    }
```

> 我们知道$()获取的是一个jQuery对象，我猜测是这么实现的。然后，不幸的是会报错，栈溢出！因为调用这个函数的时候会new一个对象，然而这个对象又需要调用本身，就是jQuery一直调用自身，然后内存瞬间膨胀，堆栈溢出。<br>
> 那应该怎样处理呢？先想一下jQuery对象的各种方法属性都封装在哪儿了？直接在构造函数里面封装吗？不是，这是通过this.property的方法来声明的，而this指的是当前对象，每次实例化都需要声明一次。实际上继承是更好的方法。<br>
> 首先需要设计一个原型
```javascript
jQuery.prototype = {
    init:function(){

    },
    css:function(){

    }
}
```
> 然后再让每次生成的对象都会继承此原型上的方法和属性，这就是每次**new**的时候需要怎么new了。<br>
> 我们知道通过直接**new jQuery**的方法会死循环堆栈溢出。我们应该找到一个替代他的构造方法，而且此构造方法的原型是自己设计的原型，这个构造方法我们用原型中的init方法。那思路就简单了。
```javascript
var jQuery = function(){
    return new jQuery.prototype.init();
}
jQuery.prototype.init.prototype = jQuery.prototype = {
    init:function(){

    },
    css:function(){

    }
}
```
#### $.fn是什么

> 它实际上就是jQuery的原型

```javascript
jQuery.fn = jQuery.prototype = {
    init:function(){

    },
    css:function(){

    }
}
```

> 整体看一下代码
```javascript
var jQuery = function(){
    return new jQuery.prototype.init();
}

jQuery.fn = jQuery.prototype = {
    init:function(){

    },
    css:function(){

    }
}

jQuery.fn.init.prototype = jQuery.fn;

global.$ = global.jQuery = jQuery;

```

#### $.ajax()又是怎么来的
> 我们知道了$().css()的实现方式，我们见过这种这种调用方式:**$.ajax()**，这又是怎么实现的？<br>
> 这就是jQuery的**代码管理和维护方式**了，jQuery的代码管理方式有两种，一种是原型上的属性和方法（$().css()），一个是jQuery本身的属性和方法（$.ajax()），下面我们来看一下jQuery的扩展模块。ajax实际上是对jQuery本身的扩展。

```javascript
jQuery.extend = jQuery.fn.extend = function(){
    var len = arguments.length;
    var target = arguments[0]||{}; //逻辑或保证代码能运行
    var i = 1;
    if(typeof target !=="object" || typeof target !== "function") //说明不是引用类型和函数，是数值类型
    {
        target = {};
    }

    if(i==len) //传参个数为1，对当前jQuery本身扩展
    {
        target = this; //把对象赋值jQuery对象
        i--;
    }
    for(;i<len;i++){
        for(const key in arguments[i]){
            if(!target.hasOwnProperty[key]){
                const element = arguments[i][key];
                target[key] = element;
            }
        }
    }
}

// 重头戏来了，ajax的实现方式
jQuery.extend({
    ajax:function(){

    }
});
// 通过$.ajax()就可以调用了
```
#### 全部代码
```javascript
/*
 * @Author: limingyue 
 * @Date: 2018-01-04 21:10:13 
 * @Last Modified by: limingyue
 * @Last Modified time: 2018-01-04 23:46:48
 */
(function (global) {
    // 局部变量
    var jQuery = function () {   //看作构造函数
        // return new jQuery(); //1 内存 实例对象 2调用     死循环！！！
        // this.  是给每个对象扩展 通过原型测试最正确
        return new jQuery.prototype.init()
    }

    // $().css()管理
    jQuery.fn = jQuery.prototype = {
        // 封装
        init:function () {   //方法——构造函数
            
        },
        css:function () {
            
        }
    }

    // 工具函数 extend 管理维护代码
    jQuery.extend = jQuery.fn.extend = function () {
        var len = arguments.length;
        var target = arguments[0]||{};
        var i = 1;
        if(typeof target !== "object"||typeof target !== "function"){
            target = {};
        }
        // 参数为一个
        if(i==len){
            target = this;
            i--;
        }
        // 任意对象
        for(;i<len;i++){
            for (const key in arguments[i]) {
                if (!target.hasOwnProperty(key)) {
                    const element = arguments[i][key];
                    target[key] = element;
                    
                }
            }
        }
    }

    // $.ajax管理模块
    jQuery.extend({
        ajax:function () {
            
        }
    });
    jQuery.fn.init.prototype = jQuery.fn;
    global.$ = global.jQuery = jQuery;
})(this);
// console.log(jQuery);        //window.jQuery
// console.log($())   堆栈溢出
console.log($().css)
console.log($.fn)
```

### 说在最后

> 写的可能有些错误，也有可能是本人的理解错误，如果发现有何不妥之处请在评论区留言。