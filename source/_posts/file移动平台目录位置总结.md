---
title: File移动平台目录位置总结
tags:
  - File目录
id: '1001'
categories:
  - - AS3探究
date: 2014-05-12 23:56:20
---

在移动开发时，我们往往会疑惑该用File的哪个目录来进行文件存储和访问等，今天特意总结一下： 这是Adobe官方给出的Andriod和IOS平台的目录信息：

平台

目录类型

典型的文件系统位置

Android

应用程序

/data/data/

应用程序存储

/data/data/air.applicationID/filename/Local Store

缓存

/data/data/applicationID/cache

桌面

/mnt/sdcard

文档

/mnt/sdcard

临时

/data/data/applicationID/cache/FlashTmp.randomString

用户

/mnt/sdcard

iOS

应用程序

/var/mobile/Applications/uid/filename.app

应用程序存储

/var/mobile/Applications/uid/Library/Application Support/applicationID/Local Store

缓存

/var/mobile/Applications/uid/Library/Caches

桌面

不可访问

文档

/var/mobile/Applications/uid/Documents

临时

/private/var/mobile/Applications/uid/tmp/FlashTmpNNN

用户

不可访问

  以下是File个静态目录在Android和IOS上的情况：

Android

iOS

File.applicationDirectory

通过 URL 只读（非本机路径）

只读

File.applicationStorageDirectory

可用

可用

File.cacheDirectory

可用

可用

File.desktopDirectory

SDCard 的根目录

不可用

File.documentsDirectory

SDCard 的根目录

可用

File.userDirectory

SDCard 的根目录

不可用

File.createTempDirectory()

可用

可用

File.createTempFile()

可用

可用

  友情提示：博主一直用的File.documentsDirectory，非常好用！ 延伸阅读：http://help.adobe.com/zh\_CN/as3/dev/WS5b3ccc516d4fbf351e63e3d118666ade46-7fe4.html