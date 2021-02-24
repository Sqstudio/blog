---
title: iOS 7.1下itms-services在线安装失败的解决方法
tags:
  - ios
  - ipa
  - itms-services
  - 七牛云盘
id: '955'
categories:
  - - 移动开发
date: 2014-03-15 11:58:13
---

iOS 7.1正式版发布了，之前使用itms-services://URL方式在线安装ipa文件的方法却失效了，点击的时候报错为：“无法安装应用程序，因xxx.com的证书无效”（错误界面见下图），这应该怎么解决呢？ 其实iOS 7.1修改了manifest.plist文件的访问协议，之前可以通过http协议访问，在iOS 7.1之后必须使用https协议方式访问。 [![ios7.1_https](http://qxu2059920095.my3w.com/blog/wp-content/uploads/2014/03/ios7.1_https.jpg)](http://qxu2059920095.my3w.com/blog/wp-content/uploads/2014/03/ios7.1_https.jpg)       比如之前的链接代码为： itms-services://?action=download-manifest&url=**http**://example.com/manifest.plist 在iOS 7.1之后，就需要修改为： itms-services://?action=download-manifest&url=**https**://example.com/manifest.plist 需要一个SSL证书才能够实现在线安装ipa文件的功能。   //-------------------  延伸： 关于https设置 ----------------------------------

> 1.upload your app.plist to dropbox 2.get shared link of app.plist, like https://www.dropbox.com/s/qgknrfngaxazm38/app.plist 3.replace www.dropbox.com with dl.dropboxusercontent.com in the link, like https://dl.dropboxusercontent.com/s/qgknrfngaxazm38/app.plist 4.write your download.html like <a href="itms-services://?action=download-manifest&url=https://dl.dropboxusercontent.com/s/qgknrfngaxazm38/app.plist">INSTALL!!</a> 5.upload the download.html to dropbox 6.get shared link of download.html, like https://www.dropbox.com/s/gnoctp7n9g0l3hx/download.html 7.replace www.dropbox.com with dl.dropboxusercontent.com in the second link as well, like https://dl.dropboxusercontent.com/s/gnoctp7n9g0l3hx/download.html Now, visit https://dl.dropboxusercontent.com/s/gnoctp7n9g0l3hx/download.html in your device, you can install the app like before.   这个是通过上传dropbox来解决，事实上，只要找个可以以https方式共享外链的网盘，把plist文件上传就可以了！

 

> \-----------------2014.06.23 关于https文件，最近发现 [七牛网盘](https://portal.qiniu.com/signup?code=3lmlh7sa3lt8y)支持ssl，可以搞定！ 链接：[七牛云盘](https://portal.qiniu.com/signup?code=3lmlh7sa3lt8y "七牛") 这个网盘貌似专门为开发者服务的，不想其他的一大堆广告什么的，总之有点不一样。

  相关文章：  [在未越狱iPhone上安装测试版ipa](http://blog.sqstudio.com/other/ios/920.html)