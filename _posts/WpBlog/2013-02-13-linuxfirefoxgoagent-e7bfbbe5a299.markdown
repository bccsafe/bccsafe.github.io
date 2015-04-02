---
author: bccsafe
comments: true
date: 2013-02-13 12:04:47+00:00
layout: post
slug: linuxfirefoxgoagent-%e7%bf%bb%e5%a2%99
title: linux+firefox+goagent 翻墙
wordpress_id: 141
categories:
- wp_linux相关
post_format:
- 图像
tags:
- linux+firefox+goagent
---

系统换成了BT5  翻墙还是需要的   于是开始了百度之旅...

先是找到了firefox+ssh+autoproxy的方法

1. 先去  [http://www.cjb.net/](http://www.cjb.net/)  申请SSH帐号

2. sudo ssh -D [127.0.0.1:7070](http://127.0.0.1:7070/) USERNAME@HOSTNAME，USERNAME 就是你自己注册的用户名，HOSTNAME填216.194.70.6

3. firefox去安装autoProxy这款插件

具体请参照 [http://www.rosoo.net/a/201107/14732.html](http://www.rosoo.net/a/201107/14732.html)

不得不说网速慢得要死 

youtube看个视频  缓冲了不知道多久  终于有个声音发了出来 然后就没了 画面就从来没出现过

这能忍？

windows下我用的是chrome+goagent的方法

BT5默认是firefox 我就去百度了firefox+goagent的方法  果然有

步骤大致与windows下相同

1. 申请Google Appengine并创建appid  [http://code.google.com/intl/zh-CN/appengine/](http://code.google.com/intl/zh-CN/appengine/)

2. 下载Python版的Google App Engine SDK [https://developers.google.com/appengine/downloads?hl=zh-CN#Google_App_Engine_SDK_for_Python](https://developers.google.com/appengine/downloads?hl=zh-CN#Google_App_Engine_SDK_for_Python)   解压到google_appengine的文件夹中

3. 下载goagent    [http://code.google.com/p/goagent/](http://code.google.com/p/goagent/)   解压到goagent的文件夹中 再把这个文件夹移动到google_appengine中去

4. 修改goagent/local/proxy.ini文件中的[gae]下的appid=你的appid

5. 进入google_appengine目录然后执行上传，接着输入你的邮箱帐号和密码

		cd ./google_appengine

		python appcfg.py update goagent/server/python  

6. firefox安装AutoProxy插件，添加规则分组，添加代理规则，最后 代理服务器>编辑代理服务器>添加代理，goagent，127.0.0.1，8087，然后重启firefox你就能看到添加后的代理了，这里注意了 前面2个添加的步骤不可漏  否则无法成功添加代理

7. 开启goagent

		cd ./google_appengine/goagent/local

		python proxy.py

我使用的时候就出现了问题

      [![1](../../../../../public/Image/2013/02/12.jpg)](../../../../../public/Image/2013/02/12.jpg)

解决方法：打开goagent/local/proxy.ini  在[paas]下添加validate = 0即可

出现如下这画面就OK了  去  [http://www.youtube.com/](http://www.youtube.com/)  看看吧

     [![2](../../../../../public/Image/2013/02/2.jpg)](../../../../../public/Image/2013/02/2.jpg)


