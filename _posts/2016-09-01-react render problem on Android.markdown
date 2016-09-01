---
layout: post
title: react+redux+es6上线问题小记
categories:
- Web Developer
---

最近在折腾react全家桶，项目用`react`+`redux`+`es6`开发，写了几个页面后准备上线，发现在chrome和iphone6上工作正常，但是到了安卓机上只给一个白屏，网页的标题倒是出来了

一度崩溃后开始找工具调试，用DebugGap发现并没有报错，页面上只有一个给react插入的DOM节点，显然ReactDOM.render这个方法没有生效

google...

发现是`babel`只是将`es6`转换成`es5`，而部分安卓上浏览器对`es5`的支持还不够，比如`promise`

解决办法：`import 'babel-polyfill'`

> Babel includes a polyfill that includes a custom regenerator runtime and core.js.
> 
>This will emulate a full ES6 environment. This polyfill is automatically loaded when using babel-node and babel/register.










