---
layout: post
title: JavaScript高级程序设计 阅读笔记
categories:
- Web Developer
---

转前端5个月，意识到基础过于薄弱，买了《JavaScript高级程序设计》恶补，并把一些学到的纪录下来

* JavaScript变量

> ##### P68
> 
> 按照ECMA-262的定义，JavaScript的变量与其他语言的变量有很大区别
> 
> ECMAScript变量可能包含两种不同数据类型的值：基本类型值和引用类型值。基本类型值指的是简单的数据段，而引用类型值指那些可能由多个值构成的对象。基本类型值有Undefined、Null、Boolean、Number、String

* 检测变量类型

> ##### P72
> 
> typeof 操作符是确定一个变量是字符串、数值、布尔值，还是 undefined 的最佳工具，但在检测引用类型的值时，这个操作符的用处不大。通常，我们并不是想知道某个值是对象，而是想知道它是什么类型的对象。为此，ECMAScript提供了 instanceof 操作符
> 
> ``` JavaScript
> var s = "Nicholas";
> var b = true;
> var i = 22;
> var u;
> var n = null;
> var o = new Object();
> alert(typeof s); //string
> alert(typeof i); //number
> alert(typeof b); //boolean
> alert(typeof u); //undefined
> alert(typeof n); //object
> alert(typeof o); //object
> ```




	
 