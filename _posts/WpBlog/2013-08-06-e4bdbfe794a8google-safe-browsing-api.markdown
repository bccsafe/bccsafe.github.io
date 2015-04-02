---
author: bccsafe
comments: true
date: 2013-08-06 16:03:41+00:00
layout: post
slug: '%e4%bd%bf%e7%94%a8google-safe-browsing-api'
title: '使用Google Safe Browsing API '
wordpress_id: 295
categories:
- wp_delphi学习笔记
tags:
- Google Safe Browsing
---



Google Safe Browsing是谷歌自己的一套网址黑名单，用于鉴别网站的安全性

在chrome中使用，同时也公开了API

[https://developers.google.com/safe-browsing/?hl=zh-CN&csw=1](https://developers.google.com/safe-browsing/?hl=zh-CN&csw=1)



在这里我将用delphi来实现这个

首选先去申请一个api Key

[https://developers.google.com/safe-browsing/key_signup](https://developers.google.com/safe-browsing/key_signup)



在帮助文档中，我们看到他提供了2种方法，Get和Post

要实现GET,POST请求，那就得用WinInet或者 WinHTTP

在这里演示的是WinInet

``` delphi   
uses WinInet;

const
  sUserAgent = 'Mozilla/5.001 (windows; U; NT4.0; en-US; rv:1.0) Gecko/25250101';
  sApiKey    = '申请的api Key';
  sServer    = 'sb-ssl.google.com';
  sGetSafeBrowsing   = '/safebrowsing/api/lookup?client=delphi&apikey=%s&appver=1.5.2&pver=3.0&url=%s';
  sPostSafeBrowsing  = '/safebrowsing/api/lookup?client=delphi&apikey=%s&appver=1.5.2&pver=3.0';

function GetWinInetError(ErrorCode:Cardinal): string;
const
   winetdll = 'wininet.dll';
var
  Len: Integer;
  Buffer: PChar;
begin
  Len := FormatMessage(
  FORMAT_MESSAGE_FROM_HMODULE or FORMAT_MESSAGE_FROM_SYSTEM or
  FORMAT_MESSAGE_ALLOCATE_BUFFER or FORMAT_MESSAGE_IGNORE_INSERTS or  FORMAT_MESSAGE_ARGUMENT_ARRAY,
  Pointer(GetModuleHandle(winetdll)), ErrorCode, 0, @Buffer, SizeOf(Buffer), nil);
  try
    while (Len > 0) and {$IFDEF UNICODE}(CharInSet(Buffer[Len - 1], [#0..#32, '.'])) {$ELSE}(Buffer[Len - 1] in [#0..#32, '.']) {$ENDIF} do Dec(Len);
    SetString(Result, Buffer, Len);
  finally
    LocalFree(HLOCAL(Buffer));
  end;
end;

function URLEncode(const Url: string): string;
var
  i: Integer;
begin
  Result := '';
  for i := 1 to Length(Url) do
  begin
    case Url[i] of
      'A'..'Z', 'a'..'z', '0'..'9', '-', '_', '.':
        Result := Result + Url[i];
    else
        Result := Result + '%' + IntToHex(Ord(Url[i]), 2);
    end;
  end;
end;

function Https_Get(const ServerName,Resource : string;Var Response:AnsiString): Integer;
const
  BufferSize=1024*64;
var
  hInet    : HINTERNET;
  hConnect : HINTERNET;
  hRequest : HINTERNET;
  ErrorCode : Integer;
  lpvBuffer : PAnsiChar;
  lpdwBufferLength: DWORD;
  lpdwReserved    : DWORD;
  dwBytesRead     : DWORD;
  lpdwNumberOfBytesAvailable: DWORD;
begin
  Result  :=0;
  Response:='';
  hInet := InternetOpen(PChar(sUserAgent), INTERNET_OPEN_TYPE_PRECONFIG, nil, nil, 0);

  if hInet=nil then
  begin
    ErrorCode:=GetLastError;
    raise Exception.Create(Format('InternetOpen Error %d Description %s',[ErrorCode,GetWinInetError(ErrorCode)]));
  end;

  try
    hConnect := InternetConnect(hInet, PChar(ServerName), INTERNET_DEFAULT_HTTPS_PORT, nil, nil, INTERNET_SERVICE_HTTP, 0, 0);
    if hConnect=nil then
    begin
      ErrorCode:=GetLastError;
      raise Exception.Create(Format('InternetConnect Error %d Description %s',[ErrorCode,GetWinInetError(ErrorCode)]));
    end;

    try
      //make the request
      hRequest := HttpOpenRequest(hConnect, 'GET', PChar(Resource), HTTP_VERSION, '', nil, INTERNET_FLAG_SECURE, 0);
      if hRequest=nil then
      begin
        ErrorCode:=GetLastError;
        raise Exception.Create(Format('HttpOpenRequest Error %d Description %s',[ErrorCode,GetWinInetError(ErrorCode)]));
      end;

      try
        //send the GET request
        if not HttpSendRequest(hRequest, nil, 0, nil, 0) then
        begin
          ErrorCode:=GetLastError;
          raise Exception.Create(Format('HttpSendRequest Error %d Description %s',[ErrorCode,GetWinInetError(ErrorCode)]));
        end;

          lpdwBufferLength:=SizeOf(Result);
          lpdwReserved    :=0;
          //get the status code
          if not HttpQueryInfo(hRequest, HTTP_QUERY_STATUS_CODE or HTTP_QUERY_FLAG_NUMBER, @Result, lpdwBufferLength, lpdwReserved) then
          begin
            ErrorCode:=GetLastError;
            raise Exception.Create(Format('HttpQueryInfo Error %d Description %s',[ErrorCode,GetWinInetError(ErrorCode)]));
          end;

         if Result=200 then //read the body response in case which the status code is 200
          if InternetQueryDataAvailable(hRequest, lpdwNumberOfBytesAvailable, 0, 0) then
          begin
            GetMem(lpvBuffer,lpdwBufferLength);
            try
              SetLength(Response,lpdwNumberOfBytesAvailable);
              InternetReadFile(hRequest, @Response[1], lpdwNumberOfBytesAvailable, dwBytesRead);
            finally
              FreeMem(lpvBuffer);
            end;
          end
          else
          begin
            ErrorCode:=GetLastError;
            raise Exception.Create(Format('InternetQueryDataAvailable Error %d Description %s',[ErrorCode,GetWinInetError(ErrorCode)]));
          end;

      finally
        InternetCloseHandle(hRequest);
      end;
    finally
      InternetCloseHandle(hConnect);
    end;
  finally
    InternetCloseHandle(hInet);
  end;
end;

function Https_Post(const ServerName,Resource: String;const PostData : AnsiString;Var Response:AnsiString): Integer;
const
  BufferSize=1024*64;
var
  hInet    : HINTERNET;
  hConnect : HINTERNET;
  hRequest : HINTERNET;
  ErrorCode : Integer;
  lpdwBufferLength: DWORD;
  lpdwReserved    : DWORD;
  dwBytesRead     : DWORD;
  lpdwNumberOfBytesAvailable: DWORD;
begin
  Result  :=0;
  Response:='';
  hInet := InternetOpen(PChar(sUserAgent), INTERNET_OPEN_TYPE_PRECONFIG, nil, nil, 0);

  if hInet=nil then
  begin
    ErrorCode:=GetLastError;
    raise Exception.Create(Format('InternetOpen Error %d Description %s',[ErrorCode,GetWinInetError(ErrorCode)]));
  end;

  try
    hConnect := InternetConnect(hInet, PChar(ServerName), INTERNET_DEFAULT_HTTPS_PORT, nil, nil, INTERNET_SERVICE_HTTP, 0, 0);
    if hConnect=nil then
    begin
      ErrorCode:=GetLastError;
      raise Exception.Create(Format('InternetConnect Error %d Description %s',[ErrorCode,GetWinInetError(ErrorCode)]));
    end;

    try
      hRequest := HttpOpenRequest(hConnect, 'POST', PChar(Resource), HTTP_VERSION, '', nil, INTERNET_FLAG_SECURE, 0);
      if hRequest=nil then
      begin
        ErrorCode:=GetLastError;
        raise Exception.Create(Format('HttpOpenRequest Error %d Description %s',[ErrorCode,GetWinInetError(ErrorCode)]));
      end;

      try
        //send the post request
        if not HTTPSendRequest(hRequest, nil, 0, @PostData[1], Length(PostData)) then
        begin
          ErrorCode:=GetLastError;
          raise Exception.Create(Format('HttpSendRequest Error %d Description %s',[ErrorCode,GetWinInetError(ErrorCode)]));
        end;

          lpdwBufferLength:=SizeOf(Result);
          lpdwReserved    :=0;
          //get the response code
          if not HttpQueryInfo(hRequest, HTTP_QUERY_STATUS_CODE or HTTP_QUERY_FLAG_NUMBER, @Result, lpdwBufferLength, lpdwReserved) then
          begin
            ErrorCode:=GetLastError;
            raise Exception.Create(Format('HttpQueryInfo Error %d Description %s',[ErrorCode,GetWinInetError(ErrorCode)]));
          end;

         //if the response code =200 then get the body
         if Result=200 then
          if InternetQueryDataAvailable(hRequest, lpdwNumberOfBytesAvailable, 0, 0) then
          begin
            SetLength(Response,lpdwNumberOfBytesAvailable);
            InternetReadFile(hRequest, @Response[1], lpdwNumberOfBytesAvailable, dwBytesRead);
          end
          else
          begin
            ErrorCode:=GetLastError;
            raise Exception.Create(Format('InternetQueryDataAvailable Error %d Description %s',[ErrorCode,GetWinInetError(ErrorCode)]));
          end;

      finally
        InternetCloseHandle(hRequest);
      end;
    finally
      InternetCloseHandle(hConnect);
    end;
  finally
    InternetCloseHandle(hInet);
  end;
end;

{Get}
var
 Response     : AnsiString;
 ResponseCode : Integer;
 AUrl:string;
begin
  memo1.Lines.Add('Using GET Method');
  AUrl:='http://www.google.com/';
  try
    ResponseCode:=Https_Get(sServer,Format(sGetSafeBrowsing,[sApiKey,URLEncode(AUrl)]), Response);
    case ResponseCode of
     200: memo1.Lines.Add(Format('The queried URL (%s) is %s',[AUrl,Response]));
     204: memo1.Lines.Add(Format('The queried URL (%s) is %s',[AUrl,'legitimate']));
     400: memo1.Lines.Add('Bad Request — The HTTP request was not correctly formed.');
     401: memo1.Lines.Add('Not Authorized — The apikey is not authorized');
     503: memo1.Lines.Add('Service Unavailable — The server cannot handle the request.');
    else
         memo1.Lines.Add('Unknow response');
   end;
  except
  end;
end;

{Post}
var
 Response     : AnsiString;
 ResponseCode : Integer;
 Data         : AnsiString;
 i            : integer;
 LstUrl       : TStringList;
 UrlList      : Array of AnsiString;
begin
   memo1.Lines.Add('Using Post Method');
   setlength(UrlList,3);
   UrlList[0]:='orgsite.info';
   UrlList[1]:='http://www.google.com';
   UrlList[2]:='http://malware.testing.google.test/testing/malware/';
   //create the body request with the url to lookup
   Data:=AnsiString(IntToStr(Length(UrlList)))+#10;
   for i:= low(UrlList) to high(UrlList) do
     Data:=Data+UrlList[i]+#10;

   //make the post request
   ResponseCode:=Https_Post(sServer,Format(sPostSafeBrowsing,[sApiKey]), Data, Response);

   //process the response
   case ResponseCode of
     200:
          begin
             LstUrl:=TStringList.Create;
             try
               LstUrl.Text:=string(Response);
                for i:=0 to  LstUrl.Count-1  do
                 Writeln(Format('The queried URL (%s) is %s',[UrlList[i],LstUrl[i]]));

             finally
               LstUrl.Free;
             end;
          end;
     204: memo1.Lines.Add('NONE of the queried URLs matched the phishing or malware lists, no response body returned');
     400: memo1.Lines.Add('Bad Request — The HTTP request was not correctly formed.');
     401: memo1.Lines.Add('Not Authorized — The apikey is not authorized');
     503: memo1.Lines.Add('Service Unavailable — The server cannot handle the request.');
   else
         memo1.Lines.Add(Format('Unknow response Code (%d)',[ResponseCode]));
   end;
end;
```





测试下来有时可以，有时直接卡住，可能是因为服务器在美国的原因吧

做成线程效果会更好

DEMO下载： [http://pan.baidu.com/share/link?shareid=4112538672&uk=1982385223 ](http://pan.baidu.com/share/link?shareid=4112538672&uk=1982385223)



参考文章：[http://theroadtodelphi.wordpress.com/2011/07/11/using-the-google-safe-browsing-api-from-delphi/](http://theroadtodelphi.wordpress.com/2011/07/11/using-the-google-safe-browsing-api-from-delphi/)
