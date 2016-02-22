---
author: bccsafe
comments: true
date: 2014-02-21 08:26:45+00:00
layout: post
slug: delphi%e4%b8%ad%e7%9a%84ifthen
title: Delphi中的IfThen
wordpress_id: 465
categories:
- wp_delphi学习笔记
tags:
- IfThen
---





Delphi中的IfThen类似于C中的三目表达式，其实就是写了一个函数来实现这个功能，一个If判断即可，分别在 System.Math,  System.StrUtils单元下

``` delphi   
//unit System.Math;

function IfThen(AValue: Boolean; const ATrue: Integer; const AFalse: Integer = 0): Integer; overload; inline;
function IfThen(AValue: Boolean; const ATrue: Int64; const AFalse: Int64 = 0): Int64; overload; inline;
function IfThen(AValue: Boolean; const ATrue: UInt64; const AFalse: UInt64 = 0): UInt64; overload; inline;
function IfThen(AValue: Boolean; const ATrue: Single; const AFalse: Single = 0.0): Single; overload; inline;
function IfThen(AValue: Boolean; const ATrue: Double; const AFalse: Double = 0.0): Double; overload; inline;
function IfThen(AValue: Boolean; const ATrue: Extended; const AFalse: Extended = 0.0): Extended; overload; inline;

//unit System.StrUtils
function IfThen(AValue: Boolean; const ATrue: string; AFalse: string = ''): string; overload; inline;
```








