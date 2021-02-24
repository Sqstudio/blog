---
title: Vbox扩容VDI硬盘大小
tags:
  - mac
  - vbox
id: '906'
categories:
  - - 周边技术
date: 2013-07-06 10:20:03
---

[![vbox_logo2_gradient](http://qxu2059920095.my3w.com/blog/wp-content/uploads/2013/07/vbox_logo2_gradient.png)](http://qxu2059920095.my3w.com/blog/wp-content/uploads/2013/07/vbox_logo2_gradient.png)用vbox装的黑mac，当时动态分配的20G，现在不太够用了，扩容方法如下： 命令参考：VBoxmanage modifyhd name.vdi/vhd –-resize size 1.打开cmd，进入vbox安装目录如（D:\\Program Files (x86)\\vbox） 2.在cmd提示符下面输入VBoxmanage modifyhd E:\\mac\\mac.vdi –resize 40960

size是以M（兆）为单位的，我这里扩展为40G。执行完之后你会惊喜的发现，这个虚拟盘已经变大了。

注：在扩展容量之前，需要先把虚拟机关掉