---
layout: post
title: AngularJs1-ES6-Webpack 项目搭建
categories:
- Web Developer
---

关键字：`AngularJs1`, `ES6`, `SASS`, `Webpack`, `hot module replacement`

###一.AngularJs ES6写法

#### 路由

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
```
	





