---
title: AndroidStudio 升级配置，不只需要翻墙(Mac)
tags: []
id: '1105'
categories:
  - - Android开发
date: 2016-01-21 15:23:29
---

翻了墙还不能升级AndroidStudio，今天中午下点功夫找找原因 原来还需要在安装包的bin文件夹下studio.vmoptions加上如下配置： `配置如下： -Djava.net.preferIPv4Stack=true -Didea.updates.url=http://dl.google.com/android/studio/patches/updates.xml -Didea.patches.url=http://dl.google.com/android/studio/patches/` 或者在 info.plist文件的<key>VMOptions</key> <string>里加上这些配置，也是一样的！ 终于升级了，越来越好用了！