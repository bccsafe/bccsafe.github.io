---
author: bccsafe
comments: true
date: 2014-04-02 15:34:23+00:00
layout: post
slug: '%e6%9e%9a%e4%b8%be%e7%b1%bb%e5%9e%8b%e4%b8%8estring%e7%9b%b8%e4%ba%92%e8%bd%ac%e6%8d%a2'
title: 枚举类型与string相互转换
wordpress_id: 495
categories:
- wp_delphi学习笔记
---
``` delphi
//uses typinfo;

function StrToEnum(IStr: string): TUser_right;
begin
  Result := TUser_right(GetEnumValue(Typeinfo(TUser_right), IStr));
end;

function EnumToStr(IEnum: TUser_right): string;
begin
  Result := GetEnumName(Typeinfo(TUser_right), Ord(IEnum));
end;
```



有时候还是会用到的，备份。


