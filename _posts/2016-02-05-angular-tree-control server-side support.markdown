---
layout: post
title: 为angular-tree-control添加server-side功能
categories:
- Web Developer
---

### angular-tree-control

angular下的树形控件

主页: <a href="http://wix.github.io/angular-tree-control/" target="_blank">http://wix.github.io/angular-tree-control/</a>

Github: <a href="https://github.com/wix/angular-tree-control" target="_blank">https://github.com/wix/angular-tree-control</a>

[![angular-tree-control](../../../../../public/Image/2016/02/angular-tree-control.png)](../../../../../public/Image/2016/02/angular-tree-control.png)

---

用下来很舒服，但是不支持服务端处理数据，即打开一个树的节点时从服务器上取数据，就动手自己改了

fork下来的git地址: <a href="https://github.com/bccsafe/angular-tree-control" target="_blank">https://github.com/bccsafe/angular-tree-control</a>

实现原理大致就是在每个节点下添加一个占位符，节点展开前调用事先设置好的`onNodeRequestData`事件，ajax获取数据后替换掉占位符

<a href="http://plnkr.co/edit/CcRiGi?p=info" target="_blank">演示地址</a>

注：因为Plunker不支持图片，就用了背景色演示，丑了点...嗯