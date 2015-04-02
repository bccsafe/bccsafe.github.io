---
author: bccsafe
comments: true
date: 2013-06-20 15:40:38+00:00
layout: post
slug: delphi%e4%b8%8b%e7%bb%9d%e5%af%b9%e8%b7%af%e5%be%84%e4%b8%8e%e7%9b%b8%e5%af%b9%e8%b7%af%e5%be%84%e7%9a%84%e7%9b%b8%e4%ba%92%e8%bd%ac%e6%8d%a2
title: delphi下绝对路径与相对路径的相互转换
wordpress_id: 255
categories:
- wp_Code[Delphi]
tags:
- 相对路径
- 绝对路径
---

绝对路径与相对路径在写程序时偶尔会用到

能完成一些特定的需求

delphi中自带了绝对路径转相对路径的函数ExtractRelativePath

却没有将向对路径转绝对路径的函数，比较遗憾。

``` delphi
//uses ShlwApi
function GetAbsolutePathEx(BasePath, RelativePath:string):string;//相对路径转绝对路径
var
   Dest:array [0..MAX_PATH] of char;
begin
   FillChar(Dest,MAX_PATH+1,0);
   PathCombine(Dest,PChar(ExtractFilePath(BasePath)), PChar(RelativePath));
   Result:=string(Dest);
end;

procedure test();
begin
  showmessage(Paramstr(0));
  showmessage(ExtractRelativePath(Paramstr(0),'C:\1.exe'));
  showmessage(GetAbsolutePathEx(Paramstr(0),ExtractRelativePath(Paramstr(0),'C:\1.exe')));
end;
```




