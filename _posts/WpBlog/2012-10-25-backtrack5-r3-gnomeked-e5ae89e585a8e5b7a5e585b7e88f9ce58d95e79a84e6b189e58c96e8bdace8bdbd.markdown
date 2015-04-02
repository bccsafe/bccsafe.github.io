---
author: bccsafe
comments: true
date: 2012-10-25 15:00:15+00:00
layout: post
slug: backtrack5-r3-gnomeked-%e5%ae%89%e5%85%a8%e5%b7%a5%e5%85%b7%e8%8f%9c%e5%8d%95%e7%9a%84%e6%b1%89%e5%8c%96%e8%bd%ac%e8%bd%bd
title: BackTrack5 R3汉化
wordpress_id: 50
categories:
- wp_bt5 学习笔记
post_format:
- 图像
tags:
- BT5汉化
---

**BackTrack5 R3汉化整理**** **




**[![](../../../../../public/Image/2012/10/20121025155787619.jpg)](../../../../../public/Image/2012/10/20121025155787619.jpg)**




**backtrack5 是一款基于 ubuntu的 linux系统**

** 其中集成了大量 安全测试 渗透测试工具**


**从网上整理了点汉化BT5的资料...**


	
  1. **第一页  导航**

	
  2. **第二页  系统语言汉化**

	
  3. **第三页  菜单汉化**

	
  4. **第四页  中文输入法**

	
  5. **第五页  火狐游览器汉化**




****




**系统语言汉化**




**第一步先下载脚本文件**


**迅雷下载：**

**gnome脚本下载地址：[http://kuai.xunlei.com/d/BTMXNZSWWCIF](http://kuai.xunlei.com/d/BTMXNZSWWCIF)**

**KED脚本下载地址：[http://kuai.xunlei.com/d/BVWRUBQJYRXW](http://kuai.xunlei.com/d/BVWRUBQJYRXW)**

**备用下载：**

**gnome脚本下载地址：[http://pan.baidu.com/share/link?shareid=4279&uk=506224987](http://pan.baidu.com/share/link?shareid=4279&uk=506224987)**

**KED脚本下载地址：[http://pan.baidu.com/share/link?shareid=4280&uk=506224987](http://pan.baidu.com/share/link?shareid=4280&uk=506224987)**

**然后将下载下来的脚本文件拷贝到BT5的root目录下**

**[![](../../../../../public/Image/2012/10/1.jpg)](../../../../../public/Image/2012/10/1.jpg)**

**然后将文件压解**

**[![](../../../../../public/Image/2012/10/2.jpg)](../../../../../public/Image/2012/10/2.jpg)[![](../../../../../public/Image/2012/10/3.jpg)](../../../../../public/Image/2012/10/3.jpg)压解后文件可以在root目录下看到**

**[![](../../../../../public/Image/2012/10/4.jpg)](../../../../../public/Image/2012/10/4.jpg)**

**进入文件夹可以看到Menuchange-zh.sh 和Menuchange-en.sh  运行Menuchange-zh.sh即可汉化**

**[![](../../../../../public/Image/2012/10/5.jpg)](../../../../../public/Image/2012/10/5.jpg)**

**root@bt:~#  cd /root/bt5-gnome-MenuChange**

**root@bt:~#  ./MenuChange-zh.sh**

**打完这些 如果提示“更改完成，重启后生效”的字样...就说明成功了~~**

****


**菜单汉化**


** ****1****、****安装中文语言包：**

**root@bt:~#  apt-get install language-support-zh language-pack-zh **



** ****2****、****安装语言支持：**

**root@bt:~# apt-get install language-selector**



** ****3****、更新数据****：**



**root@bt:~# apt-get update**

**root@bt:~# apt-get upgrade **




**完****成****后操****如图****：**** **

[![](../../../../../public/Image/2012/10/11.jpg)](../../../../../public/Image/2012/10/11.jpg)

**双击****language support****进入如图界面****：**

[![](../../../../../public/Image/2012/10/无标题.jpg)](../../../../../public/Image/2012/10/无标题.jpg)




**选择****install /remove language****进到语言选择界面 如图操作：**

[![](../../../../../public/Image/2012/10/31.jpg)](../../../../../public/Image/2012/10/31.jpg)


**选择好中文语言包后 点击apply 按钮 然后选择text 进入以下界面： **




[![](../../../../../public/Image/2012/10/51.jpg)](../../../../../public/Image/2012/10/51.jpg)




**选择语言为汉语中国 然后点击apply 按钮将其应用到整个系统 **


**最重启系统就可以看到中文界面了**




**中文输入法**




** 系统汉化后安装ibus 输入法**




**root@bt:~# apt-get install ibus-pinyin  就这条命令就 OK**




** **


**用ctrl+空格键 就能召唤出ibus 输入法 **

****


**火狐游览器汉化**


**第一步：打开BT5的英文版火狐 地址栏中输入http://stage.mozilla.org/pub/mozilla.org/firefox/releases/ 回车**

**[![](../../../../../public/Image/2012/10/12.jpg)](../../../../../public/Image/2012/10/12.jpg)**

**选择最新版本 14.01 进入 选择linux-i686/**

**[![](../../../../../public/Image/2012/10/22.jpg)](../../../../../public/Image/2012/10/22.jpg)**

**选择后进入 再选择xpi/**

**[![](../../../../../public/Image/2012/10/32.jpg)](../../../../../public/Image/2012/10/32.jpg)**

**进入后然后选择zh-CN.xpi 看清楚后缀是.xpi**

**[![](../../../../../public/Image/2012/10/41.jpg)](../../../../../public/Image/2012/10/41.jpg)**

**然后双击提示安装 选择allow 重启浏览器 记住还没完 **

**重启后在地址栏中输入about:config 回车**

**[![](../../../../../public/Image/2012/10/52.jpg)](../../../../../public/Image/2012/10/52.jpg)**

**然后在过滤器里面输入：general.useragent.locale**

**[![](../../../../../public/Image/2012/10/61.jpg)](../../../../../public/Image/2012/10/61.jpg)**

**然后双击此项general.useragent.locale;zh-CN**

**[![](../../../../../public/Image/2012/10/71.jpg)](../../../../../public/Image/2012/10/71.jpg)**

**再弹出的对话框里输入"zh-CN",点确定**

**[![](../../../../../public/Image/2012/10/8-1024x630.jpg)](../../../../../public/Image/2012/10/8.jpg)**

**然后重启火狐 此时你看见的火狐就已经汉化了 ！**
