---
author: bccsafe
comments: true
date: 2014-03-17 14:49:41+00:00
layout: post
slug: delphi-%e8%b7%a8%e8%bf%9b%e7%a8%8b%e4%bc%a0%e8%be%93%e6%95%b0%e6%8d%ae
title: delphi 跨进程传输数据
wordpress_id: 483
categories:
- wp_Code[Delphi]
tags:
- CreateFileMapping
- 内存映射文件
---

最近需要用到，就写了一个类方便以后调用，主要就利用了内存映射文件来实现

``` delphi    
unit UnitShareMemory;

//Writed By BccSafe
//http://www.bccsafe.com

interface

uses
  Winapi.Windows, System.StrUtils;

type
  PSharedData = ^TSharedData;
  TSharedData = Record
    MyStr: string[255];
    //MyStr: array [0..1024] of Char;
    MyInt: Integer;
  end;

  TShareMemory = class
  private
    FMapHandle:THandle;
    FMutexHandle:THandle;
    FLockMutexHandle:THandle;
    FData: PSharedData;
    FMapName: string;
    FMutexName: string;
    FLockMutexName: string;
    FMapExists: Boolean;
    FOpened: Boolean;
    FConnected: Boolean;
    FLocked: Boolean;
    procedure CloseShareMap;
    procedure CreateShareMap;
    procedure OpenShareMap;
    procedure ConnectShareMap;
    procedure DisconnectShareMap;
    function ReadMyStr: string;
    procedure SetMystr(const Value: string);
    function ReadMyInt: Integer;
    procedure SetMyInt(const Value: Integer);
    procedure SetLocked(const Value: Boolean);
  public
    constructor Create(MapName, MutexName, LockMutexName: string);
    destructor Destroy;override;
  published
    property MyStr: string read ReadMyStr write SetMyStr;
    property MyInt: integer read ReadMyInt write SetMyInt;
    property Locked: Boolean read FLocked write SetLocked;
  end;

const
  DefaultShareMapName = 'BccSafe_ShareMemory_FileMap';
  DefaultShareMutex = 'BccSafe_ShareMemory_Mutex';
  DefaultLockMemMutex = 'BccSafe_LockShareMemory_Mutex';

implementation

{ TShareMemory }

constructor TShareMemory.Create(MapName, MutexName, LockMutexName: string);
begin
  inherited Create;
  FLocked          := False;
  FMapExists       := False;
  FConnected       := False;
  FOpened          := False;
  FMapName         := IfThen(MapName = '', DefaultShareMapName, MapName);
  FMutexName       := IfThen(FMutexName = '', DefaultShareMutex, FMutexName);
  FLockMutexName   := IfThen(FLockMutexName = '', DefaultLockMemMutex, LockMutexName);

  FMutexHandle:= CreateMutex(nil, True, Pchar(FMutexName));
  if GetLastError <> ERROR_ALREADY_EXISTS then
    CreateShareMap else OpenShareMap;
end;

destructor TShareMemory.Destroy;
begin
  CloseShareMap;
  CloseHandle(FMutexHandle);
  inherited Destroy;
end;

procedure TShareMemory.CreateShareMap;
begin
  FMapHandle:= CreateFileMapping($FFFFFFFF, nil, PAGE_READWRITE, 0,
                 SizeOf(TSharedData), PChar(FMapName));

  if FMapHandle <> 0 then
  begin
    FMapExists:= True;
    ConnectShareMap;
  end else Halt;
end;

procedure TShareMemory.OpenShareMap;
begin
  FMapHandle:= OpenFileMapping(FILE_MAP_ALL_ACCESS,
                 False, PChar(FMapName));

  if FMapHandle <> 0 then
  begin
    FOpened:= True;
    ConnectShareMap;
  end;// else Halt;
end;

function TShareMemory.ReadMyInt: Integer;
begin
  if FData <> nil then
  begin
    Locked:= True;
    Result:= FData.MyInt;
    Locked:= False;
  end;
end;

function TShareMemory.ReadMyStr: string;
begin
  if FData <> nil then
  begin
    Locked:= True;
    Result:= FData.MyStr;
    Locked:= False;
  end;
end;

procedure TShareMemory.SetMyInt(const Value: Integer);
begin
  if FData <> nil then
  begin
    Locked:= True;
    FData.MyInt:= Value;
    Locked:= False;
  end;
end;

procedure TShareMemory.SetMystr(const Value: string);
begin
  if FData <> nil then
  begin
    Locked:= True;
    FData.MyStr:= Value;  //Strcopy(FData.MyStr ,pchar(Value));
    Locked:= False;
  end;
end;

procedure TShareMemory.SetLocked(const Value: Boolean);
begin
  if Value then
  begin
    FLockMutexHandle:= CreateMutex(nil, True, PChar(FLockMutexName));
    if FLockMutexHandle <> 0 then
      FLocked:= True
    else FLocked:= False;
  end else
  begin
    FLockMutexHandle:= OpenMutex(MUTEX_ALL_ACCESS, False, PChar(FLockMutexName));
    if FLockMutexHandle  <> 0 then
    begin
      CloseHandle(FLockMutexHandle);
      FLockMutexHandle:= 0;
      FLocked:= False;
    end;
  end;
end;

procedure TShareMemory.CloseShareMap;
begin
  if FMapHandle <> 0 then
  begin
    CloseHandle(FMapHandle);
    FMapHandle:= 0;
  end;
end;

procedure TShareMemory.ConnectShareMap;
begin
  if Not (FMapExists or FOpened) then Exit;
  FData:= MapViewOfFile(FMapHandle,
            FILE_MAP_ALL_ACCESS, 0, 0, SizeOf(TSharedData));

  if FData <> nil then FConnected:= True;
end;

procedure TShareMemory.DisconnectShareMap;
begin
  if FData <> nil then
  begin
    UnmapViewOfFile(FData);
    FData:= nil;
    FConnected:= False;
  end;
end;

end.
```





调用方法

``` delphi    
//ShareMemory: TShareMemory;

//project1
  ShareMemory:= TShareMemory.Create('', '', '');
  ShareMemory.str:= 'test';//写入数据

//project2
   ShareMemory:= TShareMemory.Create('', '', '');
  showmessage(ShareMemory.str);//读取数据
```


