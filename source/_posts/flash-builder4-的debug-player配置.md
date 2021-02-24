---
title: Flash Builder4 的debug Player配置
tags:
  - debug
  - flash builder 4
  - flash player 10
  - 配置
id: '247'
categories:
  - - AS3探究
date: 2011-03-05 15:16:47
---

关于Flash builder 4中flash player debug 版本的配置，在经历了一番折腾后，和大家分享下我遇到的问题和解决办法： 首先安装flash builder 4 ，正常安装后，就开始安装debug版本的flash player，官方下载网址：[http://www.adobe.com/support/flashplayer/downloads.html](http://www.adobe.com/support/flashplayer/downloads.html) **![](http://qxu2059920095.my3w.com/blog/wp-content/uploads/2011/03/debug-player.png "debug player")Windows***   [![Download](http://www.adobe.com/images/icons/download.gif)Download the Windows Flash Player 10.2 ActiveX control content debugger (for IE)](http://download.macromedia.com/pub/flashplayer/updaters/10/flashplayer_10_ax_debug.exe) (EXE, 2.99MB)
*   [![Download](http://www.adobe.com/images/icons/download.gif)Download the Windows Flash Player 10.2 Plugin content debugger (for Netscape-compatible browsers)](http://download.macromedia.com/pub/flashplayer/updaters/10/flashplayer_10_plugin_debug.exe) (EXE, 2.95MB)
*   [![Download](http://www.adobe.com/images/icons/download.gif)Download the Windows Flash Player 10.2 Projector content debugger](http://download.macromedia.com/pub/flashplayer/updaters/10/flashplayer_10_sa_debug.exe) (EXE, 6.36MB)
*   [![Download](http://www.adobe.com/images/icons/download.gif)Download the Windows Flash Player 10.2 Projector](http://download.macromedia.com/pub/flashplayer/updaters/10/flashplayer_10_sa.exe) (EXE, 5.37MB)
在 Windows 下有四个版本的flash player，第一个是给IE浏览器装的 debug版本，是个安装文件，第二个应该是给其他浏览器安装的，也是安装文件，第三个是 独立的 Flash player 的debug版本，这个不是安装文件，而是个exe可执行文件，最后一个是非debug版本的Flash player。 前两个版本就不用说了，下载好直接安装就可以了，主要说第三个版本，下载后可以放到Flash builder4的安装目录下的 player/win 文件夹里，说到这里就要注意了：

> 找一个swf文件，右键打开方式，找到flash player 10 debug.exe,更改swf文件的默认打开程序！