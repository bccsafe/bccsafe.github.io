---
layout: post
title: DcefBrowser疑难问题 
categories:
- DcefBrowser
---

## 黑屏问题

因为部分集成显卡版本太老或是不支持，导致webkit渲染失败，手动添加参数，关闭硬件渲染

    
    procedure OnbeforeCmdLine(const processType: ustring;
    const commandLine: ICefCommandLine);
    begin
      commandLine.AppendSwitch('disable-gpu'); 
    end;
     
    CefOnBeforeCommandLineProcessing := OnbeforeCmdLine;
    
## 如何让DcefBrowser支持摄像头

当前版本需要手动添加参数，可能以后dcef3会提供接口甚至回调事件

    
    procedure OnbeforeCmdLine(const processType: ustring;
    const commandLine: ICefCommandLine);
    begin
      commandLine.AppendSwitch('enable-media-stream'); 
      commandLine.AppendSwitch('enable-usermedia-screen-capturing'); 
    end;
    
    CefOnBeforeCommandLineProcessing := OnbeforeCmdLine;

## 如何让DcefBrowser支持Flash

需要用到pepperflash插件，由于git上不能上传这类文件，还有版权问题，就未添加到TDcefBrowser里

[下载地址](http://pan.baidu.com/s/1kT9nz3h)

    
      if not CefLoadLibDefault then
        Exit;
    
      CefAddWebPluginPath(ExtractFilePath(Paramstr(0)) +
        'PepperFlash\pepflashplayer.dll');
      CefRefreshWebPlugins();

## 解决语言环境问题


单纯的设置CefLocale := 'zh-CN'有时并不能解决问题，JS获取的navigator.language的确为zh-CN，但很多网页通过HTTP_ACCEPT_LANGUAGE来判断语言，例如QQ邮箱，因此我们需要在OnBeforeResourceLoad事件中做相应的设置

    
    procedure TMainForm.DcefBrowserBeforeResourceLoad(const PageIndex: Integer;
      const browser: ICefBrowser; const frame: ICefFrame;
      const request: ICefRequest; var CancelLoad: Boolean);
    var
      hm: ICefStringMultimap;
    begin
      if Not request.IsReadOnly then
      begin
        hm := TCefStringMultimapOwn.Create;
        request.GetHeaderMap(hm);
        hm.Append('Accept-Language', 'zh-CN');
        request.SetHeaderMap(hm);
      end;
    end;






