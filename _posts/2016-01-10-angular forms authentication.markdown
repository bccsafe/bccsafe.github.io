---
layout: post
title: Angular表单验证
categories:
- 前端笔记
---

接触Angular不久，项目中很多需要提交表单，google了些资料，在这里记录下来
	
* 常用验证指令

	1. `required` 
	2. `ng-minlength`
	3. `ng-maxlength`
	4. `ng-pattern`
	5. `type="email"`
	6. `type="number"`
	7. `type="url"`
	
* Angular 的表单属性

	1. `$valid` 是否验证通过
	2. `$invalid` 是否验证未通过
	3. `$pristine`	是否没有输入
	4. `$dirty`	是否有输入
	5. `$submitted` 是否提交了表单
	6. `$error` 包含了某个表单所有的验证信息以及表单是否合法
	
* css 属性

	1. `ng-pristine`
	2. `ng-dirty`
	3. `ng-valid`
	4. `ng-invalid`

* 禁用浏览器自带的验证功能

	form标签上加上`novalidate`属性，可以禁用浏览器自带的验证功能，改用Angular的
	
* 配合ng-message实现实时错误提示

	需要用到angular-messages.js, module中引用`ngMessages`这个模块
	
	账号名必须输入，长度最长10位
	
```html
<form name="myForm" ng-submit="doSubmit(myForm.$valid)" novalidate>
    <div>
        <label>账号</label>
        <div ng-class="{'has-error' : myForm.name.$invalid && myForm.name.$dirty ||myForm.$submitted}">
            <input type="text" name="name" ng-model="param.name" ng-trim="true" ng-maxlength=10 required>  
            <div ng-messages="myForm.name.$error" ng-if="myForm.$submitted || myForm.name.$dirty && myForm.name.$invalid">
                <div ng-message="required">账号不能为空</div>
                <div ng-message="maxlength">账号最长不能超过10个字</div> 
            </div>
        </div> 
        <button type="submit">注册</button>
    </div>
</form> 	
``` 
	
```javascript
var app = angular.module("app", ['ngMessages']); 

app.controller('controller', function ($scope) {  
	$scope.param = { 
 		name: ""
	}

	$scope.doSubmit = function(isValid){
 
	}
})
```  
	
<a href="http://plnkr.co/edit/SYMpQP?p=info" target="_blank">演示地址</a> 	
* 自定义验证 - 对比两个input值是否相等 
	
```html
<form name="myForm" ng-submit="doSubmit(myForm.$valid)" novalidate>
  <div>
      <label>密码</label>
      <div ng-class="{'has-error' : myForm.pwd1.$invalid && myForm.pwd1.$dirty ||myForm.$submitted}">
          <input type="text" name="pwd1" ng-model="param.pwd1" ng-trim="true" required> 
      </div>   
  </div>
  <div>
      <label>再次输入密码</label>
      <div ng-class="{'has-error' : myForm.pwd2.$invalid && myForm.pwd2.$dirty ||myForm.$submitted}">
          <input type="text" name="pwd2" ng-model="param.pwd2" ng-trim="true" required compare-to="param.pwd1"> 
          <div ng-messages="myForm.pwd2.$error" ng-if="myForm.$submitted || myForm.pwd2.$dirty && myForm.pwd2.$invalid"> 
              <div ng-message="compareTo">两次密码不一致</div> 
          </div>
      </div>   
  </div> 
  <button type="submit">注册</button>
</form> 
```
	
```javascript
var app = angular.module("app", ['ngMessages']); 

app.directive("compareTo", function() {
 	return {
     	require: "ngModel",
     	scope: {
         	otherModelValue: "=compareTo"
     	},
     	link: function(scope, element, attributes, ngModel) { 
	ngModel.$validators.compareTo = function(modelValue) { 
             	return modelValue == scope.otherModelValue;
         	};            
	scope.$watch("otherModelValue", function() {
             	ngModel.$validate();
         	});
     	}
 	};
});

app.controller('controller', function ($scope) {  
	$scope.param = { 
 		pwd1: "",
 		pwd2: ""
	}

	$scope.doSubmit = function(isValid){
 
	}
})
``` 
	
<a href="http://plnkr.co/edit/vQK3JS?p=info" target="_blank">演示地址</a> 
	
* 自定义验证 - ajax后台验证是否重名


```html	
<form name="myForm" ng-submit="doSubmit(myForm.$valid)" novalidate>
  <div>
    <label>账号</label>
    <div ng-class="{'has-error' : myForm.name.$invalid && myForm.name.$dirty ||myForm.$submitted}">
      <input type="text" name="name" ng-model="param.name" ng-trim="true" ng-maxlength=10 check-name="param.name" required>
      <div ng-messages="myForm.name.$error" ng-if="myForm.$submitted || myForm.name.$dirty && myForm.name.$invalid">
        <div ng-message="param.name">该账号已被注册</div>
      </div>
    </div> 
    <button type="submit">注册</button>
  </div>
</form>
```
	
```javascript
var app = angular.module("app", ['ngMessages']); 

app.directive("checkName",function() {
	return {
   		require: "ngModel", 
   		link: function(scope, element, attributes, ngModel) {
	element.bind('keyup', function() { 
       		ngModel.$setValidity(attributes.checkName, ngModel.$modelValue == "test");  
       		//ajax
       		}); 
   		}
	};
});

app.controller('controller', function ($scope) {  
	$scope.param = { 
 		name: ""
	}  

	$scope.doSubmit = function(isValid){
 
	}
})
```
	
<a href="http://plnkr.co/edit/LCAaka?p=info" target="_blank">演示地址</a>  


