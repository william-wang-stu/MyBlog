---
title: JavaScript必知33个概念系列：作用域
mathjax: false
copyright: true
comment: true
date: 2019-11-14 15:57:37
tags:
- JavaScript
- 33 JS Concept
categories:
- JavaScript
photo: http://07imgmini.eastday.com/mobile/20181002/84ec6a264fd6d97d019837dd57c75021_wmk.jpeg
---

{% note primary %}
函数作用域，块级作用域，词法作用域。
{% endnote %}

<!-- more -->

---

## 前言

我对于作用域一直处于一种感性的认识，有问题也是凭感觉去看。今天就打破这面镜子，看看镜子后面到底是什么。

先说一下我现在的认知水平：
首先函数作用域我是知道的，函数体内部就是函数作用域；
然后块级作用域我大概知道，花括号内部？if、else、for这些？
最后词法作用域？没听过。。

接下来我们来探索一下这些术语（terminology）的语义吧。

## 作用域

作用域往往和变量挂钩，在谈作用域之前，我们先来看看声明变量的三种方法以及他们之间的区别。

### var、let、const

#### var

**用`var`声明的对象是属于函数作用域（function scope）的，如果你不在函数作用域下声明，那这个变量就是*更高级函数作用域*或者全局的（global scope）。**

看几个例子：

```javascript
function setWidth(){
    var width = 100;
    console.log(width);
}
width;
// Returns:
// Uncaught ReferenceError: width is not defined
```
因为`width`是在函数作用域（`setWIdth`函数）下用`var`声明的，所以他属于函数作用域，因此全局作用域下访问不到`width`


```javascript
var age = 100;
if (age > 12){
  var dogYears = age * 7;
  console.log(`You are ${dogYears} dog years old!`);
}
dogYears;
// returns:
700
```
因为用`var`声明的变量`dogYears`不是在函数作用域下声明的，因此在本例中会当做全局作用域。

#### let和const
**用`let`和`const`声明的变量是属于块级作用域的，而不是函数作用域。**

**块级作用域就是大括号“{}”之间的部分。**

类似的下面这个例子，我用`let`代替`var`来声明`dogYears`变量

```javascript
var age = 100;
if (age > 12){
  let dogYears = age * 7;
  console.log(`You are ${dogYears} dog years old!`);
}
dogYears;
// returns:
// Uncaught ReferenceError: dogYears is not defined
```
因为使用`let`定义`dogYears`变量，那么`dogYears`变量属于块级作用域，即if的作用域。

### 函数作用域（function scope）
> 函数体内部叫函数作用域。

- 用`var`声明的对象是属于函数作用域（function scope）的，如果你不在函数作用域下声明，那这个变量就是*更高级函数作用域*或者全局的（global scope）

### 块级作用域（block scope）
> 大括号“{}”之间称为块级作用域，当然函数作用域也是块级作用域。

- 用`let`和`const`声明的变量是属于块级作用域的，而不是函数作用域。

### 词法作用域（lexical scope）

**词法作用域就是可以通过源代码来看出哪个变量属于哪个作用域，不会根据函数被调用的上下文改变，词法和静态（static）可以看做是一样的。**

## 相关话题

### 变量提升（变量声明提升，variables declaration hoisted）
> JavaScript将变量的声明提升到该变量的直接作用域下的开始处，变量的赋值并不提升。函数定义也会进行提升，因此作用域下函数定义在前在后没多大区别。


**仅对于`var`声明的变量进行提升，对`const`、`let`声明的变量不适用。**

看个例子：
```javascript
function f() {
    console.log(bar);  // undefined
    var bar = 'abc';
    console.log(bar);  // abc
}
```

函数`f()`的实现就好像如下定义一样：

```javascript
function f() {
    var bar;
    console.log(bar);  // undefined
    bar = 'abc';
    console.log(bar);  // abc
}
```

### 关于“`var`是函数作用域的”引起的问题和解决办法

举个例子，如下代码，假如说你不想在if语句后面使用`tmp`变量或者说你有一个新的同名的`tmp`变量，那你如何解决这个问题呢？

```javascript
function f() {
    if (condition) {
        var tmp = ...;
        ...
    }
    // tmp still exists here
    // => not what we want
}
```

- 方法一：使用`let`（因为`let`是块级作用域的
- 方法二：立即执行函数（利用`var`是函数作用域的，`ES5`时是一个常见的编程模式。

```javascript
function f() {
    if (condition) {
        (function () {  // open block
            var tmp = ...;
            ...
        }());  // close block
    }
}
```



## Reference

- [The Difference Between Function and Block Scope in JavaScript](https://medium.com/@josephcardillo/the-difference-between-function-and-block-scope-in-javascript-4296b2322abe)
- [What the Heck is Lexical Scope? (JavaScript)](https://www.youtube.com/watch?v=GhNA0r10MmA)
- [Chapter 16. Variables: Scopes, Environments, and Closures](http://speakingjs.com/es5/ch16.html)
