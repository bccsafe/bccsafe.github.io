---
layout: post
title: DcefBrowser详细介绍
categories:
- DcefBrowser 
---

##使用注意事项

dcef3是多进程架构(CefSingleProgress := False)，而DcefBrowser是对dcef3的再次封装，因此很多下面所提到的注意事项也同样适用于dcef3
	
1. 对DcefBrowser的设置必须在工程文件(.dpr)中的Application.Initialize前，常规设置如下

		CefSingleProcess := False;      
		CefRemoteDebuggingPort := 9000;
		if not CefLoadLibDefault then
		  Exit; 
    
		 Application.Initialize;
		Application.CreateForm(TMainForm, MainForm);
		Application.Run; 	
2. 在主窗口FormCreate中需要添加以下代码，处理多窗口切换的焦点问题

    
		procedure TMainForm.FormCreate(Sender: TObject);
		begin
		  DcefBrowser1.Options.MainFormWinHandle := Handle;
		end;
    
		procedure TMainForm.WndProc(var Message: TMessage);
		begin
		  if Assigned(DcefBrowser1) then
		    DcefBrowser1.MainFormWndProc(Message, Handle);
		 
		  inherited WndProc(Message);
		end;

 	
3. 请一定不要在事件中直接访问外部的变量，特别是TMainForm，你可以使用SendMessage/PostMessage的方式实现进程间通讯   	
4. TBrowserPage 有PageIndex和PageID两个属性，在不变动TAB顺序的情况下PageIndex=PageID，当你自己管理TAB页的时候，访问Page的正确方式应该是TDcefBrowser.GetPageByID  
5. 不管是拖控件或是动态创建的，需要在TForm.Destory事件中释放TDcefBrowser

##设置介绍

### 1)dcef3相关设置

* CefCache   
	缓存目录
	
* CefUserAgent Useragent  
	用户代理
  
* CefLocale   
	语言
		
* CefLogSeverity   
	超过该等级的日志将被记录
  
* CefPackLoadingDisabled   
	禁用加载包文件资源和语言环境
	
* CefSingleProcess   
	单/多进程模式
   	
* CefRemoteDebuggingPort   
	远程调试端口 最新版本中远程调试已去除
	
以上这些都是ceflib内的全局变量，对他们的修改需要工程文件而且在CefLoadLibDefault之前

	procedure test;
	begin
		CefCache := 'C:\CefCache\';
		//CefUserAgent := 'Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/37.0.2062.94 Safari/537.36';
		CefLocale := 'zh-CN';
		CefLogSeverity := LOGSEVERITY_DISABLE;
		CefPackLoadingDisabled := False;
		CefSingleProcess := False;
		CefRemoteDebuggingPort := '9000';
    
		if not CefLoadLibDefault then Exit;
	end;
    
		
	
### 2)TDcefBrowser相关设置

* 是否自动完成下载    
	TDcefBrowser.Options.AutoDown
	
* 默认下载路径     
	TDcefBrowser.Options.DownLoadPath
    	
* 是否弹出新窗口      
	DcefBrowser.Options.PopupNewWindow
  	
* 是否允许使用F12开发者工具      
	TDcefBrowser.Options.DevToolsEnable
  	
* 所有标签关闭后是否终止APP      
	TDcefBrowser.Options.ExitPagesClosed


**以上只是简单列举了一些常用的设置，更多的见`CefLib.pas`和`dcefb_Options.pas`**

##事件介绍

### 1）DcefBrowser新增的事件
 
* OnPageLoadingStateChange                 
	当页面载入状态(是否在载入，能否前进/后退)发生改变时调用此方法 

*  OnPageStateChange    
	当页面状态发生(URL，标题，Hint)变化时调用此方法
     	
* OnPageAdd   
	当添加新标签时调用此方法
 
* OnPageClose   
	当删除标签时调用此方法	
	ClosePageID，ShowPageID请使用TDcefBrowser.GetPageByID	并检查对象是否为nil，ShowPageID可能为-1即不显示新的标签页
	
* <s>OnDownloadUpdated</s> 	
	<s>执行下载后会多次调用此方法，开始下载，更新下载进度，结束下载都有调用</s>
	
	因为错误的设计，该方法已在`V 0.08`移除，当前版本的OnDownloadUpdated只是在dcef3的基础上加上了`PageIndex`参数 

### 2）在dcef3的事件的基础上添加PageIndex属性，大致方法不变  	
  * OnLoadStart   
	当页面开始加载时调用此方法，会多次调用 
	
  * OnLoadEnd   
	当页面加载完成后调用此方法，会多次调用
	
  * OnLoadError   
	当页面加载错误时调用此方法 
		
  * OnBeforeBrowse   
	访问网站前调用此方法 
		
  * OnPreKeyEvent，OnKeyEvent   
	键盘输入时调用此方法
	
  * OnBeforeResourceLoad，OnGetResourceHandler   
	资源载入前/时调用此方法
		
  * OnResourceRedirect   
	页面重定向时调用此方法
  
  * OnBeforeContextMenu，OnContextMenuCommand，OnContextMenuDismissed  
	右击菜单前/时/后调用此方法
	  	
  * OnJsdialog   
  	打开JS对话框时调用此方法
  
  * OnBeforeUnloadDialog   
	确认是否关闭页面时调用此方法
  	
  * OnDialogClosed   
	对话框关闭时调用此方法
	
  * OnPluginCrashed   
	插件崩溃时调用此方法
  	
  * OnBeforePluginLoad   
	载入插件前调用此方法
  	
  * OnGetAuthCredentials   
	验证用户是调用此方法
  	
  * OnConsoleMessage   
	获得控制台消息时调用此方法