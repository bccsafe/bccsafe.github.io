---
layout: post
title: DcefBrowser简介
categories:
- DcefBrowser
---

## 为什么要编写TDcefBrowser
 	
1. dcef3提供的TChromium不能用于编写多标签的游览器(顺便指出个误区，很多人上来会在OnBeforePopup事件中截获新标签页的URL然后再动态创建个TChromium，这是不可取的！这样只能处理GET请求，POST请求就挂了)，所以在TDcefBrowser中对此进行了处理，统一管理原生的cef3窗口与弹出的窗口	
2. 编写游览器需要考虑到很多方面，如果你没有在dcef3提供的事件中编写对应的代码，有些WEB上的功能将有问题，对此我结合我的开发经验在TDcefBrowser内置了各类解决方案，例如JS alert，window.onbeforeunload等等	
3. 对游览器需要的状态量开放了接口，方便使用，例如网页能否倒退/前进，网页标题的变化等
4. 处理了几个dcef3的坑，某些特定情况下的异常关闭等问题

## 如何编译TDcefBrowser Demo

1. 从[Git](https://github.com/bccsafe/DcefBrowser) 上下载控件源代码
2. 打开`DcerBrowser\bin\Win32\`，按照`readme.txt`上的提示下载cef3 lib至该目录
3. 打开DcefBrowser\packages\DcefBrowser.dproj，Compile - install 控件就安装完成了	
4. 打开DcefBrowser\demos\Simple\SimpleDcefB.dproj `Run！`  



TDcefBrowser使用Delphi XE6开发

很遗憾目前暂时不支持低版本的Delphi，用到了一些高版本的语法