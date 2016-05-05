---
layout: post
title: AngularJS控制器继承自另一控制器
categories:
- Web Developer
---

AngularJS里控制器继承，常用的就是作用域嵌套作用域。默认情况下，当前作用域中无法找到某个属性时，就会在父级作用域中进行查找，若找不到直至查找到$rootScope。

但有些情况下，rootScope下就是我们的controller，不可能将大量的公用属性方法写到rootScope里去。

比如说有多个类似的页面，都有面包屑，搜索栏，工具栏，表格等元素，面包屑表格这种元素考虑做成directive，那么必然会有许多类似的配置需要从controller传到组件里去，也会产生很多工具类方法用于处理数据等，这时候在每个页面的controller里重复写相同的代码显然很难看，就需要用到继承。

在StackOverflow上找到了解决方案，原来AngularJS已经考虑到这种情况了，提供了$controller


```javascript
var app = angular.module('angularjs-starter', []); 
app.controller('ParentCtrl ', function($scope) {
  // I'm the sibling, but want to act as parent
});
app.controller('ChildCtrl', function($scope, $controller) {
  $controller('ParentCtrl', {$scope: $scope}); //This works
});
``` 


<a href="http://stackoverflow.com/questions/18461263/can-an-angularjs-controller-inherit-from-another-controller-in-the-same-module" target="_blank">StackOverflow链接</a>