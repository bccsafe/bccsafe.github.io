---
layout: post
title: ECMAScript创建对象的若干方法
categories:
- Web Developer
---

以下内容均整理自《JavaScript高级程序设计》第六章 面向对象的程序设计

PS 读完这一章真的熟悉颇多，之前自己编写工具类的时候就很疑惑，只是简单的百度了解下，这次的则较为系统。

* 工厂模式
 
> ``` JavaScript
> function createPerson(name, age, job){>         var o = new Object();>         o.name = name;>         o.age = age;>         o.job = job;>         o.sayName = function(){>             alert(this.name);>         };> 		  return o;>  }>     var person1 = createPerson("Nicholas", 29, "Software Engineer");>     var person2 = createPerson("Greg", 27, "Doctor");
> ```

* 构造函数模式

> ``` JavaScript
> function Person(name, age, job){>     this.name = name;>     this.age = age;>     this.job = job;>     this.sayName = sayName;> }> function sayName(){>     alert(this.name);> }> var person1 = new Person("Nicholas", 29, "Software Engineer");> var person2 = new Person("Greg", 27, "Doctor");
> ```

* 原型模式

> ``` JavaScript
> function Person(){> }> Person.prototype = {>     constructor: Person,>     name : "Nicholas",>     age : 29,>     job : "Software Engineer",>     friends : ["Shelby", "Court"],>     sayName : function () {>         alert(this.name);> 	  }>  };> var person1 = new Person();> var person2 = new Person();> person1.friends.push("Van");> alert(person1.friends);    //"Shelby,Court,Van"> alert(person2.friends);    //"Shelby,Court,Van"> alert(person1.friends === person2.friends);  //true
> ```

* 组合使用构造函数模式和原型模式

> ``` JavaScript
> function Person(name, age, job){
> 	this.name = name;
> 	this.age = age;
> 	this.friends = ["Shelby", "Court"];
> }
> Person.prototype = {
> 	constructor: Person,
> 	sayName: function() {
> 		alert(this.name);
> 	}
> }
> var person1 = new Person("Nicholas", 29, "Software Engineer");
> var person2 = new Person("Greg", 27, "Doctor");
> person1.friends.push("Van");> alert(person1.friends);    //"Shelby,Count,Van"> alert(person2.friends);    //"Shelby,Count"> alert(person1.friends === person2.friends);//false> alert(person1.sayName === person2.sayName);//true
> ```

* 动态原型模式

> ``` JavaScript
> function Person(name, age, job){
> 		//属性> 		this.name = name;> 		this.age = age;> 		this.job = job;
> 		//方法
> 		if (typeof this.sayName != "function"){
> 			Person.prototype.sayName = function(){
> 				alert(this.name);
> 			}; 
> 		}￼￼> }￼￼> var friend = new Person("Nicholas", 29, "Software Engineer");
￼￼> friend.sayName();
> ```

* 寄生构造函数模式

> ``` JavaScript
> function Person(name, age, job){>         var o = new Object();>         o.name = name;>         o.age = age;>         o.job = job;>         o.sayName = function(){>             alert(this.name);>         };> 		  return o;
> }> var friend = new Person("Nicholas", 29, "Software Engineer");> friend.sayName();  //"Nicholas"
> ```

* 稳妥构造函数模式

> ``` JavaScript
> function Person(name, age, job){>         //创建要返回的对象>         var o = new Object();>         //可以在这里定义私有变量和函数
> 		  //...>         //添加方法>         o.sayName = function(){>             alert(this.name);>         };> 		  return o;
> }> var friend = Person("Nicholas", 29, "Software Engineer");> friend.sayName();  //"Nicholas"
> ```

### 优缺点

/ | 对象识别(constructor) | 多个实例共享同一个function | function不在全局作用域，具有封装性 | 构造函数能传递初始化参数 | 多个实例有各自的属性，不共享
------------ | ------------- | ------------
工厂模式 | No  | No | Yes | Yes | Yes |
构造函数模式 | Yes  | Yes | No | Yes | Yes |
原型模式 | Yes<br>(需要设置constructor值)  | Yes | Yes | No | No 
组合使用构造函数模式和原型模式 | Yes  | Yes | Yes | Yes | Yes |
动态原型模式 | Yes  | Yes | Yes | Yes | Yes | 

### 总结

上表可以看出`组合使用构造函数模式和原型模式`和`动态原型模式`是较为完美的方案，前者更常用，而`寄生构造函数模式`和`稳妥构造函数模式`则是为了解决特殊的问题而提出的，不在讨论范围内，前者为了在已有对象上添加一个额外方法，并创建构造函数；后者则是安全方面的考虑。
	
 