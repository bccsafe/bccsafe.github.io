---
layout: post
title: POST请求 request payload 转 form data
categories:
- 前端笔记
---

后端方面用了token验证，每个请求的header里都要求加上token

前端项目里主要用到了Angular, 少部分用了jQuery

以下是两个方案的解决办法

* jQuery

```javascript
function initJqueryAjaxSetting() {
	jQuery.ajaxSetup( {  
	    type: "POST", 
	    "contentType": "application/x-www-form-urlencoded",
        headers: {
	        'userToken': tools.loginManager.getToken()
	    },  
		success: function( data, textStatus, jqXHR ){
	        tools.loginManager.checkResponse(data);
	    }
	});
}
``` 

* Angular

```javascript
app.factory('UserInterceptor', ["$q","$rootScope",function ($q,$rootScope) {
    return {
        request:function(config){
            config.headers["userToken"] = tools.loginManager.getToken();  
            return config;
        },
        response: function(response) {
            var deferred = $q.defer();  
            tools.loginManager.checkResponse(response.data);  
            deferred.resolve(response);  
            return deferred.promise; 
        }
    };
}]);  

app.config(function ($stateProvider, $httpProvider) {
    $httpProvider.defaults.headers.post['Content-Type'] = 'application/x-www-form-urlencoded';
    $httpProvider.defaults.transformRequest = function(data) {
        var str = [];
        for (var p in data)
            str.push(encodeURIComponent(p) + "=" + encodeURIComponent(data[p]));
        return str.join("&");
    };
    $httpProvider.defaults.headers.post['userToken'] = tools.loginManager.getToken();     $httpProvider.interceptors.push('UserInterceptor'); 
}); 
```