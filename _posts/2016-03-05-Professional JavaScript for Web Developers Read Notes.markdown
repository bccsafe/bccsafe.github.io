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
> typeof 操作符是确定一个变量是字符串、数值、布尔值，还是 undefined 的最佳工具 
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
> 
> 但在检测引用类型的值时，这个操作符的用处不大。通常，我们并不是想知道某个值是对象，而是想知道它是什么类型的对象。为此，ECMAScript提供了 instanceof 操作符
> 
> ``` JavaScript
> alert(person instanceof Object); // 变量 person 是 Object 吗？
> alert(colors instanceof Array); // 变量 colors 是 Array 吗？
> alert(pattern instanceof RegExp); // 变量 pattern 是 RegExp 吗？
> ```

* 垃圾回收

> ##### P78
> 
> JavaScript 具有自动垃圾收集机制，也就是说，执行环境会负责管理代码执行过程中使用的内存。垃圾收集方式有`标记清楚`和`引用计数`，前者最常用。
> 
> 性能问题与内存管理。垃圾收集器是周期性运行的，确保占用最少的内存可以让页面获得更好的性能。而优化内存占用的最佳方式，就是为执行中的代码只保存必要的数据。一旦数据不再有用，最好通过将其值设置为 null 来释放其引用——这个做法叫做`解除引用`。

* Array类型

> ##### P86
> 
> 除了 Object 之外，Array 类型恐怕是 ECMAScript 中最常用的类型了
> 
> * `push` 接收任意数量的参数，把它们逐个添加到数组末尾，并返回修改后数组的长度。
> * `pop` 从数组末尾移除最后一项，减少数组的 length 值，然后返回移除的项
> * `shift` 移除数组中的第一个项并返回该项，同时将数组长度减1
> * `reverse` 反转数组项的顺序并返回
> * `sort` 按升序排列数组项并返回
> * `concat` 在没有给 concat()方法传递参数的情况下，它只是
复制当前数组并返回副本。如果传递给 concat()方法的是一或多个数组，则该方法会将这些数组中的
每一项都添加到结果数组中。如果传递的值不是数组，这些值就会被简单地添加到结果数组的末尾。
> * `slice` slice()方法可以
接受一或两个参数，即要返回项的起始和结束位置。在只有一个参数的情况下，slice()方法返回从该
参数指定位置开始到当前数组末尾的所有项。如果有两个参数，该方法返回起始和结束位置之间的项—
—但不包括结束位置的项。注意，slice()方法不会影响原始数组。可用于删除，插入，替换。
> * `indexOf` 接收两个参数：要查找的项和（可选的）表示查找起点位置的索引。从数组的开头（位置 0）开始向后查找
> * `lastIndexOf` 接收两个参数：要查找的项和（可选的）表示查找起点位置的索引。从数组的末尾开始向前查找。
> * `every` 对数组中的每一项运行给定函数，如果该函数对每一项都返回 true，则返回 true。
> * `filter` 对数组中的每一项运行给定函数，返回该函数会返回 true 的项组成的数组。
> * `forEach` 对数组中的每一项运行给定函数。这个方法没有返回值。
> * `map` 对数组中的每一项运行给定函数，返回每次函数调用的结果组成的数组。
> * `some ` 对数组中的每一项运行给定函数，如果该函数对任一项返回 true，则返回 true。
> 
> ```JavaScript
> var numbers = [1,2,3,4,5,4,3,2,1];
> var everyResult = numbers.every(function(item, index, array){
>  return (item > 2);
> });
> alert(everyResult); //false
> var someResult = numbers.some(function(item, index, array){
>  return (item > 2);
> });
> alert(someResult); //true 
> var filterResult = numbers.filter(function(item, index, array){
>  return (item > 2);
> });
> alert(filterResult); //[3,4,5,4,3] 
> var mapResult = numbers.map(function(item, index, array){
>  return item * 2;
> });
> alert(mapResult); //[2,4,6,8,10,8,6,4,2] 
> ```
> 
> * `reduce` 迭代数组的所有项，然后构建一个最终返回的值。从数组的第一项开始，逐个遍历
到最后。
> * `reduceRight` 迭代数组的所有项，然后构建一个最终返回的值。从数组的最后一项开始，向前遍历到第一项。
> 
> ```JavaScript
> var values = [1,2,3,4,5];
> var sum = values.reduce(function(prev, cur, index, array){
>  return prev + cur;
> });
> alert(sum); //15 
> ```






	
 