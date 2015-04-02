---
author: bccsafe
comments: true
date: 2014-11-26 06:10:22+00:00
layout: post
slug: delphi%e8%b0%83%e7%94%a8%e7%99%be%e5%ba%a6%e7%9f%ad%e9%93%be%e6%8e%a5api
title: delphi调用百度短链接API
wordpress_id: 640
categories:
- wp_Code[Delphi]
tags:
- 短链接
---

调用百度给出的短链接API

``` delphi	
//uses idhttp, system.json;


function GetTinyUrl(Const Url: string; Var TingUrl, ErrorMsg: string): Boolean;
var
  IdHTTP: TIdHTTP;
  vJson: TJSONObject;
  JsonText: string;
  Params: TStrings;
Const
  ApiUrl = 'http://dwz.cn/create.php';
begin
  Result := False;
  ErrorMsg := '';
  TingUrl := '';
  Params := TStringList.create;
  IdHTTP := TIdHTTP.create(nil);
  IdHTTP.AllowCookies := True;
  IdHTTP.HTTPOptions := [hoForceEncodeParams];
  IdHTTP.ProtocolVersion := pv1_1;
  try
    try
      Params.Add('url=' + Url);
      JsonText := IdHTTP.Post(ApiUrl, Params);
      vJson := TJSONObject.ParseJSONValue(TEncoding.UTF8.GetBytes(JsonText), 0)
        as TJSONObject;
      if vJson.getValue('status').Value = '0' then
      begin
        Result := True;
        TingUrl := vJson.getValue('tinyurl').Value;
      end
      else
        ErrorMsg := vJson.getValue('err_msg').Value;
    except
      Result := False;
      ErrorMsg := 'Error In Http';
    end;
  finally
    Params.Free;
  end;
end;


//调用
//var
//  TinyUrl, ErrMsg: string;
//begin
//  if GetTinyUrl('www.bccsafe.com', TinyUrl, ErrMsg) then
//    Memo1.Lines.Add(TinyUrl)
//  else
//    Memo1.Lines.Add(ErrMsg);
//end;
```

</br>
百度API文档：

>	5.怎样调用百度短网址API？
>	
>	生成短网址
>	
>	请求：向dwz.cn/create.php发送post请求，发送数据包括url=长网址
>	
>	返回：json格式的数据
>	
>	status!=0 出错，查看err_msg获得错误信息（UTF-8编码）
>	
>	成功，返回生成的短网址 tinyurl字段
>	
>	自定义短网址
>	
>	请求：向dwz.cn/create.php发送post请求，发送数据包括url=长网址&alias=自定义网址
>	
>	返回：json格式的数据
>	
>	Status!=0 出错，查看err_msg获得错误信息（UTF-8编码）
>	
>	成功，返回生成的短网址 tinyurl字段
>	
>	显示原网址
>	
>	请求：向dwz.cn/query.php发送post请求，发送数据包括tinyurl=查询的短地址
>	
>	返回：json格式的数据	status!=0 出错，查看err_msg获得错误信息（UTF-8编码）
>		成功，返回原网址 longurl字段
