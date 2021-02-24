---
title: Android开发入门之从环境搭建到HelloWorld
tags:
  - Air for Android
  - Android
  - 环境配置
id: '390'
categories:
  - - AS3探究
date: 2011-07-08 17:09:25
---

这两天开始学Android开发，今天折腾了一天总算完成了Android的HelloWorld，并且在Android模拟器顺利启动。只可恨网速实在不给力，一直徘徊在几K到几十K之间。 一、环境搭建 通过AIR For Android来开发Android应用，现在比较成熟完整的开发环境有两种： 1.Flash builder4.5。FB4.5在新建项目的时候增加了两个类型Flex手机项目和ActionScript手机项目，手机项目同时支持Android和iPhone。 2.Flash CS5.5。在FlashCS5.5中已经支持新建Android项目。 当然之前还有Flashbuilder4.0或者FlashCS5配上插件开发之类的，相信FB4.5和FlashCS5.5出来以后，那些过渡期的插件都要被逐渐淘汰了。   除了开发环境还需要用下载AndroidSDK，安装Android模拟器，我选择的是Android2.2版的，也是从这个版本之后，Android才支持AIR2.5的。当然如果你自己有Android手机就再好不过了，模拟器有些功能是没法模拟的，而且速度比较慢，尤其是开机的时候。 Android 2.2 SDK 正式版下载地址（居然被和谐，官方网站需要翻墙）：  [http://developer.android.com/sdk/android-2.2.html](http://developer.android.com/sdk/android-2.2.html) 还有个 google的地址：http://dl.google.com/android/android-sdk\_r10-windows.zip 安装过程，网上很多图文教程，我就不多说了，只要网速够给力，神马都是浮云。 [](../wp-content/uploads/2011/05/Android.png)[![](http://qxu2059920095.my3w.com/blog/wp-content/uploads/2011/05/Android1.png "Android")](http://qxu2059920095.my3w.com/blog/wp-content/uploads/2011/05/Android1.png) 二、实例制作 实例我是用FlashCS5.5制作的，直接新建Android项目即可，在发布的时候可以直接生成Android的安装包apk格式。 说下我遇见的几个问题： 1.发布的时候要注意两个选项，在Android的部署类型上一个是设备发行，一个模拟器发行，要根据自己的测试环境发布相应的格式。 2.如果是用Flash Builder开发的话，也可以用命令行来打包生成apk文件，打包命令如： adt -package -target apk-emulator -storetype pkcs12 -keystore android.p12 Test.apk Test-app.xml Test.swf 3.可以给你的Android模拟器建立个快捷方式，但是直接启动AndroidSDK/tools下的emulator.exe是不行的，在桌面快捷方式的属性/目标那一栏后面加上 @2.2，其中2.2是新建模拟器时的名字，如此就可以启动Android模拟器，当然通过命令行也可以。 暂时就写这么多，这次的没有实例代码，有兴趣的朋友自己找找下载地址安装下软件配置下环境，欢迎留言讨论！