---
layout: post
title: DcefBrowser与JS交互
categories:
- DcefBrowser
---

简单演示了与JS交互的过程，因为多进程架构，在DoTest过程里如果要操作窗体的话，建议使用SendMessage/PostMessage

    
    type
      TCustomRenderProcessHandler = class(TCefRenderProcessHandlerOwn)
      protected
        procedure OnWebKitInitialized; override;
      end;
    
      TDcefb_Extension = class
        class procedure DoTest(Msg: string);
      end;
    
    class procedure TDcefb_Extension.DoTest(Msg: string);
    begin
      ShowMessage(Msg);
    end;
    
    procedure TCustomRenderProcessHandler.OnWebKitInitialized;
    begin
      TCefRTTIExtension.Register('Dcefb_Test', TDcefb_Extension);
    end;
    




工程文件内添加

    
      CefRenderProcessHandler := TCustomRenderProcessHandler.Create;
      if not CefLoadLibDefault then
        Exit;




测试代码

		DcefBrowser1.ExecuteJavaScript('Dcefb_Test.DoTest("TestStr");');


