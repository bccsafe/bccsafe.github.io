---
layout: post
title: ios10，Android6下pushState导致的微信分享签名失败问题
categories:
- Web Developer
---

react写的webapp，在微信里用js sdk发现在ios 10，Android6 下签名挂了，一直提示`invalid signature`，但是低版本的android就可以

研究了下发现是pushState导致的，签名时我们生成签名时用的url和微信取出来的url不一致导致的，比如进页面是home页，然后进了一个详情页，此时我们需要调用分享接口，这个时候我们认为url是详情页的url，但微信认为是home页的url，这就导致了问题

解决办法就是进入页面的时候将url保存起来，分享的时候用

> 保存进入页面最初的URL，假设为INIT_URL
> 
> 根据客户端的不同：
> 
> 安卓：在准备分享前(或发生URL跳转后)使用当前URL进行wx.config， 如果失败，则尝试使用INIT_URL注册
>   
> iOS：在准备分享前(或发生URL跳转后)使用INIT_URL进行wx.config， 如果失败，则尝试使用当前URL注册

参考：<a href="https://segmentfault.com/q/1010000002520634">https://segmentfault.com/q/1010000002520634</a>














