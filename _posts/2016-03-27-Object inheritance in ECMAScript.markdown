---
layout: post
title: ECMAScript继承对象的若干方法
categories:
- Web Developer
---

以下内容均整理自《JavaScript高级程序设计》第六章 面向对象的程序设计

* 原型链
 
> ``` JavaScript
> function SuperType(){
>     this.property = true;
> }￼
> SuperType.prototype.getSuperValue = function(){
>     return this.property;
> };
> function SubType(){
>     this.subproperty = false;
> }
> //继承了 SuperType
> SubType.prototype = new SuperType();
> SubType.prototype.getSubValue = function (){
>     return this.subproperty;
> };
> var instance = new SubType();
> alert(instance.getSuperValue());//true
> ```

> ``` JavaScript
> function SuperType(){
>     this.colors = ["red", "blue", "green"];
> }
> function SubType(){
> //继承了 SuperType
>     SuperType.call(this);
> }
> var instance1 = new SubType();
> instance1.colors.push("black");
> alert(instance1.colors);    //"red,blue,green,black"
> var instance2 = new SubType();
> alert(instance2.colors);    //"red,blue,green"
> ```


* 借用构造函数

> ``` JavaScript
> function SuperType(){
>     this.colors = ["red", "blue", "green"];
> }
> function SubType(){
> //继承了 SuperType
>     SuperType.call(this);
> }
> var instance1 = new SubType();
> instance1.colors.push("black");
> alert(instance1.colors);    //"red,blue,green,black"
> var instance2 = new SubType();
> alert(instance2.colors);    //"red,blue,green"
> ```

> ``` JavaScript
> function SuperType(name){
>     this.name = name;
> }
> function SubType(){
> //继承了 SuperType,同时还传递了参数 SuperType.call(this, "Nicholas");
> //实例属性
>     this.age = 29;
> }
> var instance = new SubType();
> alert(instance.name);    //"Nicholas";
> alert(instance.age);     //29
> ```


* 组合继承

> ``` JavaScript
> function SuperType(name){
> 	this.name = name;
> 	this.colors = ["red", "blue", "green"];
> }
> SuperType.prototype.sayName = function(){
>     alert(this.name);
> };
> function SubType(name, age){
> //继承属性 SuperType.call(this, name);
>     this.age = age;
> }
> //继承方法
> SubType.prototype = new SuperType(); 
> SubType.prototype.constructor = SubType; 
> SubType.prototype.sayAge = function(){
>     alert(this.age);
> };
> var instance1 = new SubType("Nicholas", 29);
> instance1.colors.push("black");
> alert(instance1.colors);//"red,blue,green,black"
> instance1.sayName();//"Nicholas";
> instance1.sayAge();//29
> var instance2 = new SubType("Greg", 27);
> alert(instance2.colors);//"red,blue,green"
> instance2.sayName();//"Greg";
> instance2.sayAge();//27
> ```

* 原型式继承

> ``` JavaScript
> function object(o){
> 	function F(){} 
> 	F.prototype = o;
> 	return new F();
}
> var person = {
>     name: "Nicholas",
>     friends: ["Shelby", "Court", "Van"]
> };
> var anotherPerson = object(person);
> anotherPerson.name = "Greg";
> anotherPerson.friends.push("Rob");
> var yetAnotherPerson = object(person);
> yetAnotherPerson.name = "Linda";
> yetAnotherPerson.friends.push("Barbie");
> alert(person.friends);   //"Shelby,Court,Van,Rob,Barbie"
> ```

> ``` JavaScript
> var person = {
>     name: "Nicholas",
>     friends: ["Shelby", "Court", "Van"]
> };
> var anotherPerson = Object.create(person);
> anotherPerson.name = "Greg";
> anotherPerson.friends.push("Rob");
> var yetAnotherPerson = Object.create(person);
> yetAnotherPerson.name = "Linda";
> yetAnotherPerson.friends.push("Barbie");
> alert(person.friends); //"Shelby,Court,Van,Rob,Barbie"
> ```


* 寄生式继承

> ``` JavaScript
> function createAnother(original){ 
> 	var clone = object(original); //通过调用函数创建一个新对象
> 	clone.sayHi = function(){ //以某种方式来增强这个对象
> 		alert("hi");
> 	};
> 	return clone; //返回这个对象
> }
> var person = {
>     name: "Nicholas",
>     friends: ["Shelby", "Court", "Van"]
> };
> var anotherPerson = createAnother(person);
> anotherPerson.sayHi(); //"hi"
> ```

* 寄生组合式继承

> ``` JavaScript
> function SuperType(name){
>     this.name = name;
>     this.colors = ["red", "blue", "green"];
> }
> SuperType.prototype.sayName = function(){
>     alert(this.name);
> };
> function SubType(name, age){
>     SuperType.call(this, name);
>     this.age = age;
> }
> SubType.prototype = new SuperType();
> SubType.prototype.constructor = SubType;
> SubType.prototype.sayAge = function(){
>     alert(this.age);
> };
> ```

> ``` JavaScript
> function inheritPrototype(subType, superType){
>     var prototype = object(superType.prototype);
>     prototype.constructor = subType;
>     subType.prototype = prototype;
> }
> function SuperType(name){
>     this.name = name;
>     this.colors = ["red", "blue", "green"];
> }
> SuperType.prototype.sayName = function(){
>     alert(this.name);
> };
> function SubType(name, age){
>     SuperType.call(this, name);
>     this.age = age;
> }
> inheritPrototype(SubType, SuperType);
> SubType.prototype.sayAge = function(){
>     alert(this.age);
> }
> ```


### 总结

`原型式继承`可以在不必预先定义构造函数的情况下实现继承,其本质是执行对给定对象的浅复制。而复制得到的副本还可以得到进一步改造。

`组合继承`是JavaScript中最常用的继承模式，instanceof 和 isPrototypeOf()都能够用于识别基于组合继承创建的对象。

`寄生式继承`与原型式继承非常相似,也是基于某个对象或某些信息创建一个对象,然后增强对象,最后返回对象。为了解决组合继承模式由于多次调用超类型构造函数而导致的低效率问题,可以将这个模式与组合继承一起使用。

`寄生组合式继承`集寄生式继承和组合继承的优点与一身,是实现基于类型继承的最有效方式。
	
 