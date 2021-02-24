---
title: Mac下使用dex2jar
tags:
  - dex2jar
id: '1100'
categories:
  - - Android开发
date: 2016-01-18 11:04:27
---

1.去http://sourceforge.net/projects/dex2jar/ 下载一个新版本dex2jar.  目前是2.0，下载后文件名dex2jar-2.0.zip 2.可以到这里看看用法： http://sourceforge.net/p/dex2jar/wiki/Faq/ http://sourceforge.net/p/dex2jar/wiki/UserGuide/ 3.解压缩dex2jar-2.0.zip 到文件夹dex2jar-2.0 4.注意修改文件属性为可执行文件:

 chmod +x _/home/.../_d2j-dex2jar.sh  _/home/.../_d2j\_invoke.sh

5.输入命令 _   sudo sh /home/.../dex2jar-version/d2j-dex2jar.sh   /home/.../someApk.apk_ _6.在命令文件夹下面即可看到someApk.jar文件包_