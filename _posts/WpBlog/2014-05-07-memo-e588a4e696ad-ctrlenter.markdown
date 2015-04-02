---
author: bccsafe
comments: true
date: 2014-05-07 15:58:53+00:00
layout: post
slug: memo-%e5%88%a4%e6%96%ad-ctrlenter
title: Memo 判断 (ctrl)+enter
wordpress_id: 505
categories:
- wp_Code[Delphi]
---

扣扣的聊天窗口有Enter和Ctrl+Enter这两种方式发出消息，delphi下通过Memo的KeyPress和KeyDown事件也可以实现这个功能

 

PS: 最近又懒了，找回状态ING

``` delphi    
type
    TInPutMethod = (InPutEnter, InPutCtrlEnter);

{
function GetInPutMethod: TInPutMethod;
begin
  if Enter1.Checked then  //enter
    result:= InPutEnter
  else result:= InPutCtrlEnter;// ctrl + enter
end;
}

procedure SendMemoKeyDown(Sender: TObject; var Key: Word;
  Shift: TShiftState);
begin
  if Key = 13 then
  begin
    if ssCtrl in Shift then  //ctrl+enter
    begin
      if GetInPutMethod = InPutCtrlEnter then
      begin
        key:= 0;
        ButtonSend.Click;
      end;
    end
    else //enter
      if GetInPutMethod = InPutEnter then ButtonSend.Click;
  end;
end;

procedure SendMemoKeyPress(Sender: TObject; var Key: Char);
begin
  if ((Key = #13) and (GetInPutMethod = InPutEnter) ) or ((Key = #10) and (GetInPutMethod = InPutCtrlEnter)) then
    key:= #0;
end;
```


