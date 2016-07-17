---
layout: post
title: AngularJs1-ES6-Webpack 项目搭建
categories:
- Web Developer
---

关键字：`AngularJs1`, `ES6`, `SASS`, `gulp`, `Webpack`, `hot module replacement`

## 一.项目目录

```
├── gulpfile.js 存放gulp相关的配置

├── index.html 入口html

├── package.son 存放npm相关的配置

├── webpack.config.js 存放webpack相关的配置

├── css 存放scss代码

├── dist 存放打包了的js,css文件

├── js 存放es6代码

│   ├── index.js 入口文件

│   ├── compontent 组件

│       ├── module 模块组件

│       ├── CommonDirective.js 通用组件

│       └── index.js 入口文件

│   ├── config app配置

│       └── routing.js 路由

│   ├── module 模块

│       ├── test 测试模块

│       		├── index.js 入口文件

│       		├── routing.js 路由

│       		└── TestController.js 控制器

│   └── index.js 入口文件

├── view 存放html代码

│   ├── module 模块

│       ├── test 测试模块

│       		└── test.html 模版

└────
```
 
### 二.package.json

```
{
  "name": "AngularJs1-ES6-Webpack",
  "version": "0.0.1",
  "description": "A simple Demo using ES6, AngularJs1 and webpack",
  "devDependencies": {
    "angular": "~1.3.0",
    "angular-ui-router": "^0.2.14",
    "babel-core": "^6.10.4",
    "babel-loader": "^6.2.4",
    "babel-preset-es2015": "^6.9.0",
    "css-loader": "^0.23.1",
    "extract-text-webpack-plugin": "^1.0.1",
    "gulp": "^3.9.0",
    "gulp-util": "^3.0.7",
    "ng-annotate-loader": "~0.0.4",
    "node-sass": "^3.8.0",
    "raw-loader": "^0.5.1",
    "sass-loader": "^4.0.0",
    "style-loader": "^0.13.1",
    "webpack": "^1.13.1",
    "webpack-dev-server": "^1.12.1"
  },
  "author": "BccSafe",
  "license": "MIT"
}
``` 

## 三.AngularJs ES6写法

### 1)路由

```javascript
//routing.js
import TestController from './TestController.js';

export default function routing($stateProvider) {
	'ngInject';
	
    $stateProvider.state('test', {
        url: '/test',
        templateUrl: "view/module/test/test.html",
        controller: TestController
    });
}

//index.js
import 'angular';
import angularUIRouter from 'angular-ui-router';
import routing from './config/routing.js';

let myApp = angular.module("myApp", [angularUIRouter]);

myApp.config(routing);
```

### 2)Controller(Service, Factory同理)

```javascript
//TestController.js
class TestController {

    constructor($rootScope, $scope, $stateParams){
        'ngInject';

        $scope.testValue = "this value from the Class TestController"; 
    }

}

export default TestController;

//index.js
import 'angular';
import angularUIRouter from 'angular-ui-router';
let testModule = angular.module('testModule', [angularUIRouter]);

export default testModule = testModule.name;
```

### 3)Directive

```javascript
//CommonDirective
class CommonDirective {

    constructor(){
        'ngInject';

        this.template = '<div>I\'m a directive!</div>';
        this.restrict = "E";
    }

}

export default CommonDirective;

//index.js
import 'angular';
import CommonDirective from './CommonDirective.js';

let CommonCptModule = angular.module('CommonCptModule', []);

CommonCptModule.directive('commonDirective', () => new CommonDirective);

export default CommonCptModule = CommonCptModule.name;
```



	





