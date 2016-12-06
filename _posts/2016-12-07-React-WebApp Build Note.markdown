---
layout: post
title: React移动端搭脚手架日记
categories:
- Web Developer
---

来了个新项目，移动端的WebApp，打算自己搭一个脚手架出来，其中的点点滴滴会在这里记录（因为项目催得紧，一直没有时间写，拖到现在）

1. TypeScript 2 + Webpack + React

	在hot reload上用了`React-Hot-Loader3`，目前还没出正式版，参考了<a href="https://github.com/tomduncalf/typescript-react-template">https://github.com/tomduncalf/typescript-react-template</a>，其他和之前用es6没有太大差别，开发时因为需要HRM，TypeScript转换成es6，再由babel转换成es5，部署时TypeScript就直接转换成es5
	
2. webpack DLLPlugin
	
	项目中将使用react全家桶，意味着将引入大量的包，`DLLPlugin`能够前置这些依赖包的构建，来提高build和rebuild的构建效率。

3. 各种Loaders

	主要是css，sass-loader + postcss-loader，配合Autoprefixer为CSS添加浏览器特定的前缀


4. 用typescript来写redux

	参考了<a href="http://jaysoo.ca/2015/09/26/typed-react-and-redux/">http://jaysoo.ca/2015/09/26/typed-react-and-redux/</a>


大致就是这样，有些细节和待优化的点后续补上

github地址：<a href="https://github.com/bccsafe/React-WebApp-Boilerplate">https://github.com/bccsafe/React-WebApp-Boilerplate</a>














