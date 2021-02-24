---
title: 查看ipa中包含的设备UDID
tags:
  - ios
  - ipa
  - UDID
id: '1383'
categories:
  - - 移动开发
date: 2018-06-01 14:41:59
---

开发iOS的小伙伴都知道，要想把编译好的 ipa文件安装到iOS设备上，必须提前把该设备的UDID包含的ipa内的mobileprovision文件里。如何查看ipa所包含的iOS设备的UDID呢？

1.  把ipa文件后缀改为.zip，然后解压
2.  显示app的内容，找到embedded.mobileprovision文件
3.  在终端中执行命令：

cms -D -i /Users/folder/embedded.mobileprovision

即可显示出所有信息！