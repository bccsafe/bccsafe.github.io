---
author: bccsafe
comments: true
date: 2013-04-06 03:54:56+00:00
layout: post
slug: '%e8%85%be%e8%ae%af%e5%be%ae%e5%8d%9a%e4%b8%8a%e7%ba%bf%e4%bb%a3%e7%a0%81%e5%ae%9e%e7%8e%b0'
title: 腾讯微博上线代码实现
wordpress_id: 156
categories:
- wp_Code[Delphi]
tags:
- 腾讯微博上线
---

转载自小坏's Blog  [http://www.lovehuai.com/](http://www.lovehuai.com/)

 



额....最近既然都流行非域名/Ftp上线了
我就也来跟风一下吧
在疼讯围脖 发布一条 IP地址|IpEnd; 的消息
例如 
113.108.7.194|IpEnd; //这个是疼讯的服务器IP ╮(╯_╰)╭

代码如下 
额....有的小盆与看到代码肯定会问
啊 你为啥不用HTTP 的HEAD 来获取所需字节大小啊 这么低端!~~
答:
HEAD T.QQ.COM  返回的HTTP 200 不包含 Content-Length



``` delphi    
function StrStrIA(lpFirst, lpSrch: PAnsiChar): PAnsiChar; stdcall; external 'shlwapi.dll' name 'StrStrIA';

function PosA(lpSrch, lpFirst:PAnsiChar):Integer;
begin
 Result := DWORD(StrStrIA(lpFirst, lpSrch)) - DWORD(lpFirst);
end;

function Get_Ip_For_QQWeiBo():PAnsiChar;
var
  Internet :Pointer;
  OpenUrl  :Pointer;
  Buffer   :PAnsiChar;
  MemSeek  :PAnsiChar;
  szSize   :DWORD;
  szBytes  :DWORD;
begin
 Result   := Nil;
 Internet := InternetOpenA('Internet Explorer 9.0', INTERNET_OPEN_TYPE_PRECONFIG, nil, nil, 0);
 if Internet <> Nil then
 begin//这里填写你自己的围脖地址
  OpenUrl := InternetOpenUrlA(Internet, 'http://t.qq.com/darkzol_huai/mine', nil, 0, 0, 0);
  if OpenUrl <> Nil Then
  begin
   szSize := 1024 * 1024 * 5;
   Buffer := GetMemory(szSize);
   MemSeek:= Buffer;
   HttpQueryInfoA(OpenUrl, HTTP_QUERY_RAW_HEADERS_CRLF, MemSeek, szSize, szBytes);
   while InternetReadFile(OpenUrl, MemSeek, szSize, szBytes) and (szBytes > 0) do
   begin
    Dec(szSize, szBytes);
    Inc(PByte(MemSeek), szBytes);
   end;
   MemSeek := Buffer;
   szSize  := PosA('id="talkList"', MemSeek);
   Inc(PByte(MemSeek), szSize);
   szSize  := PosA('msgCnt">', MemSeek) + 8;
   Inc(PByte(MemSeek), szSize);
   szSize  := PosA('|IpEnd;', MemSeek) + 1;
   szBytes := szSize + 10;
   Result  := GetMemory(szBytes);
   lstrcpynA(Result, MemSeek, szSize);
   FreeMemory(Buffer);
   InternetCloseHandle(OpenUrl);
  end;
  InternetCloseHandle(Internet);
 end;
end;
```



看到了`InternetReadFile`- -感觉又要被杀了这函数...

测试环境应该是delphi XE2 ，高三时间紧就不测试了...

原理其实和大多数URL上线一样是获取网页源代码  

具体请参考这篇文章[http://www.bccsafe.com/?p=125](http://www.bccsafe.com/?p=125)



原作者如果不同意我转载 请联系我删除

 
