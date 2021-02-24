---
title: AndroidStudio添加assets目录
tags:
  - Android
  - Android Studio
  - assets
id: '1069'
categories:
  - - Android开发
date: 2015-09-24 19:32:33
---

1: 打开 项目名称.iml 文件. 查找这行 : option name="ASSETS\_FOLDER\_RELATIVE\_PATH" value="/src/main/assets" 2: 在“main”下建立一个子目录 "assets"  （即对应src/main/assets） 3: 把webview中要加载的文件放到这个目录中. 4: 在webview中载入，完成. 参考： http://goodidea100.com/Android/744.html http://stackoverflow.com/questions/18302603/where-to-place-assets-folder-in-android-studio