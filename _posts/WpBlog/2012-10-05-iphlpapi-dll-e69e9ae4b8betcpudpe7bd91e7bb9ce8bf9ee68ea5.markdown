---
author: bccsafe
comments: true
date: 2012-10-05 15:14:18+00:00
layout: post
slug: iphlpapi-dll-%e6%9e%9a%e4%b8%betcpudp%e7%bd%91%e7%bb%9c%e8%bf%9e%e6%8e%a5
title: iphlpapi.dll 枚举TCP/UDP网络连接
wordpress_id: 27
categories:
- wp_My Work
post_format:
- 图像
tags:
- GetExtendedTcpTable
- 枚举网络连接
---

iphlpapi.dll里有很多函数都可以用来枚举网络连接

* InternalGetTcpTable2

* InternalGetUdpTableWithOwnerPid（XP以下 包括XP）

* AllocateAndGetTcpExTableFromStack

* AllocateAndGetUdpExTableFromStack（vista以上...）

如果用这2对API的话，明显比较繁琐，先要区分系统再动态加载，于是我找到了如下两个函数

* GetExtendedTcpTable

* GetExtendedUdpTable


省去了很多麻烦，win7和XP都可以...话说上下2对API的输出的结果不一样...求指教~~ 

主要就是EnumerateConnections这个单元了，抄来的稍作修改

核心代码如下：

``` delphi
function GetTcpConnections(var ConnectionArray : TConnectionArray) : boolean; 
var 
  TcpTable : PMibTcpTableOwnerPID; 
  Size : DWORD; 
  i : Integer; 
begin 
  GetExtendedTcpTable(nil, @size, FALSE, AF_INET, TCP_TABLE_OWNER_PID_ALL, 0); 
  GetMem(TcpTable, size); 
  if GetExtendedTcpTable(TcpTable, @size, FALSE, AF_INET, TCP_TABLE_OWNER_PID_ALL, 0) = NO_ERROR then 
    begin 
      Result := TRUE; 
      for i := 0 to TcpTable^.dwNumEntries - 1 do 
        begin 
          SetLength(connectionArray, Length(connectionArray) + 1); 
          with connectionArray[Length(connectionArray) - 1] do 
            begin 
              Protocol := 'TCP';
              ConnectionState := TcpTable^.table[i].dwState; 
              LocalAddress := TcpTable^.table[i].dwLocalAddr; 
              LocalRawPort := TcpTable^.table[i].dwLocalPort; 
              RemoteAddress := TcpTable^.table[i].dwRemoteAddr; 
              RemoteRawPort := TcpTable^.table[i].dwRemotePort; 
              ProcessID := TcpTable^.table[i].dwOwningPid; 
            end; 
        end; 
    end else 
      Result := FALSE; 
  FreeMem(TcpTable); 
end; 

function GetUdpConnections(var ConnectionArray : TConnectionArray) : boolean; 
var 
  UdpTable : PMibUdpTableOwnerPID; 
  Size : DWORD; 
  i : Integer; 
begin 
  GetExtendedUdpTable(nil, @size, FALSE, AF_INET, UDP_TABLE_OWNER_PID, 0); 
  GetMem(UdpTable, size); 
  if GetExtendedUdpTable(UdpTable, @size, FALSE, AF_INET, UDP_TABLE_OWNER_PID, 0) = NO_ERROR then 
    begin 
      Result := TRUE; 
      for i := 0 to UdpTable^.dwNumEntries - 1 do 
        begin 
          SetLength(connectionArray, Length(connectionArray) + 1); 
          with connectionArray[Length(connectionArray) - 1] do 
            begin 
              Protocol := 'UDP';
              ConnectionState := 0; 
              LocalAddress := UdpTable^.table[i].dwLocalAddr; 
              LocalRawPort := UdpTable^.table[i].dwLocalPort; 
              RemoteAddress := 0; 
              RemoteRawPort := 0; 
              ProcessID := UdpTable^.table[i].dwOwningPid; 
            end; 
        end; 
    end else 
      Result := FALSE; 
  FreeMem(UdpTable); 
end;
```



完整代码下载：[http://pan.baidu.com/share/link?shareid=75114&uk=1982385223](http://pan.baidu.com/share/link?shareid=75114&uk=1982385223)

里面包含了2个版本 一个是用之前提的繁琐的方法写的 另一个是用这EnumerateConnections单元的，再加上2个纯真IP数据库...体积就大了点


[![](../../../../../public/Image/2012/10/未命名-1024x520.jpg)](../../../../../public/Image/2012/10/未命名.jpg)
