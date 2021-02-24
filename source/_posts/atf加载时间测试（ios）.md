---
title: ATF加载时间测试（IOS）
tags:
  - atf
id: '1008'
categories:
  - - 移动开发
date: 2014-06-04 18:52:46
---

最近在优化项目的资源文件atf加载上传时间，在iPhone上做了一点测试，数据共享下！ [![atf_upload_test](http://qxu2059920095.my3w.com/blog/wp-content/uploads/2014/06/atf_upload_test.jpg)](http://qxu2059920095.my3w.com/blog/wp-content/uploads/2014/06/atf_upload_test.jpg)   说明： 整图---整张640x1136的图，直接转的atf（命令参考：**png2atf -c p -q 0 -n 0,0  -i a.png -o a.atf**） 拆分成4个：即把整图分别拆分为512x1024、128x1024、512x112和128x112这个尺寸的小图。 硬件环境：iPhone4/4s/5 软件环境：FB4.7+airsdk14+Starling1.4 从上面表中可以看出在iPhone4上基本近3s的时间（我记得去年在iPhone4上测试640x960的整图也基本是这个时间，看来air+starling改进很多的样子）。目前iPhone4的用户比例大约6%（朋友提供的，数据不权威，真实情况会有浮动），WWDC2014刚刚开完，iPhone6再过一段时间也要发布了，相信iPhone4用户比例在新机发布后会更进一步降低，当然如果那个iPhone4用户依然还不愿换手机的话，那么我想你也别指望让他给你的游戏付费了。总之，目前的开发标准应该瞄准iPhone4s，确保在iPhone4s上运行流畅即可，至于iPhone4，呵呵，不崩溃就行了！ 关于安卓，一直以来，安卓的atf上传时间都要比iPhone快一些，atf品质也好一些，搞的定iPhone就不用担心安卓了！ 在PC，加载相应png格式资源，整图需200+毫秒，而拆分后的，反倒需要400+毫秒。