---
author: bccsafe
comments: true
date: 2015-02-11 16:07:26+00:00
layout: post
slug: vcl-style-bug
title: VCL Style BUG
wordpress_id: 743
categories:
- wp_delphi学习笔记
tags:
- VCL Style BUG
---

窗体上一个Tlistview，TImageList，TPopupMenu，TPopupMenu上一个TMenuItem.ImageIndex := 1;

不使用VCL Style，一切正常，如图

[![VCL Style BUG 1](../../../../../public/Image/2015/02/2.png)](../../../../../public/Image/2015/02/2.png)



上皮肤后，出现了下图的问题，应该出现的图标和文字不见了

[![VCL Style BUG 2](../../../../../public/Image/2015/02/1.png)](../../../../../public/Image/2015/02/1.png)





google查资料后，原因是XE6使用了老版本的VCL Style

下载[vcl-styles-utils](https://code.google.com/p/vcl-styles-utils/%20)后，添加Vcl.Styles.Utils.Menus单元即可解决该问题

[![Vcl Style Bug 3](../../../../../public/Image/2015/02/3.png)](../../../../../public/Image/2015/02/3.png)



我在一个新的Application上测试了下，这个BUG没有重现

具体问题在哪，我也弄明白，就这样吧

皮肤这一类的好麻烦的，不研究了
