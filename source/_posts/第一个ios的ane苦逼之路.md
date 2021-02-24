---
title: 第一个IOS的ANE苦逼之路
tags:
  - ane
  - ios
id: '909'
categories:
  - - 移动开发
date: 2013-07-06 17:14:16
---

  概要：本文只是记录作者开发ANE的心得，并不会教你一步一步制作一个ANE，不过文中附有其他教程链接，可供参考！   作为一个只会as3的aser，想折腾个ANE实在太苦逼了，所幸，今天终于折腾出一个！ [![ane](http://qxu2059920095.my3w.com/blog/wp-content/uploads/2013/07/ane.jpg)](http://qxu2059920095.my3w.com/blog/wp-content/uploads/2013/07/ane.jpg) 1.先装黑苹果，各种翻论坛，安装包已经不记得下了多少个，总是卡到这里或那里，搞了n天，终于用Vbox装上了mac 10.7（幸好之前买电脑时cpu是i5，内存又加了一根现在有6G了） 2.又在ucai.cn自学了一段时间objective-c，每天晚上看视频，学着敲代码，知道了一些基本的语法。 3.学了一段时间，看看github上别人的ANE，有的可以小改一下。 4.尝试着写ane，这中间真是各种坑，还好找到一些资料，推荐几篇不错的：

> *   [IOS ANE写法指导](http://www.iphil.cc/?p=135)
> *   [IOS ANE的坑爹之路](http://rolfzhang.com/articles/1108.html)
> *   [ANE打包工具2013版](http://plter.sinaapp.com/a/p/post/2274/)
> *   [jamesli的内支付系列](http://jamesli.cn/blog/?cat=19)
> *   [如何打包ANE](http://blog.sina.com.cn/s/blog_6471e1bb01012aql.html)

第一个ane，是做了一个获取设备mac地址的，其实实现什么功能都是浮云了，只要搞通了AS3和OC的相互调用，功能就看实际需求了！ 说起as+oc的对接，就不得不提一下 ANE的Xcode模版[xcode-template-ane](https://github.com/divijkumar/xcode-template-ane),这个可以省不少代码，不用配置，还可以直接生成ane，不过要生成ane还需要AIR SDK环境等，如果你是直接用MAC PRO开发，Flash Builder和XCode一起用，就很方便了，像我这中苦逼的用虚拟机黑苹果不敢点太快怕机器卡死的aser开发者，只用下xcode生成的.a库文件就可以了！这个模版可以帮菜鸟oc同学设置好模版，我们只需要写逻辑就行了！ 在AS3和OC之间的数据交互是用的FREObejct，对于AS3来说 还是和平常用法一样，Boolean，String等随便传，但是在oc中得到的都是FREObejct类型，需要从FREObejct中提取转换成BOOL、NSString等，同样，oc方法的返回值也需要将BOOL、NSString等类型转换成FREObejct，传给as3，在as3中可以直接得到的就是Boolean、String不需要再转换，想必AIR已经自动转换好了。FREObejct和oc中的类型的转换建议还是再封装一个类比较好，这里是[Maom](https://github.com/zmLiu)的封装的方法[ANEFun](https://github.com/zmLiu/ANEList/blob/master/AneSDK_IOS/AneSDK_IOS/AneFunction.h)，可以参考一下，[Maom](https://github.com/zmLiu)的ANEList也蛮全面，很给力！ 另外一个坑就是打包ANE，前面说过用[xcode-template-ane](https://github.com/divijkumar/xcode-template-ane)模版的话，在创建.ane目标的时候，会自动调用[generateANE.sh](https://github.com/zmLiu/ANEList/blob/master/AneSDK_IOS/AneSDK_IOS/generateANE.sh "generateANE.sh")脚本，从而自动生成ANE。在window上呢？只有命令行了，当然那个《[ANE打包工具2013版](http://plter.sinaapp.com/a/p/post/2274/)》打包工具在不会打包的时候也可以用一下，推荐还是命令行，自己写个批处理工具最好！

> 命令如下： bin/adt -package -storetype pkcs12 -keystore .p12文件名称.p12 -storepass .p12文件的密码 -target ane 需要生成的ane的名称.ane extension.xml -swc swc名称.swc -platform 平台名称 library.swf 本机扩展库 （我习惯把 bin/adt 直接改为FB安装包路径名）

打包的细节可以参考上文提到的 ： [如何打包ANE](http://blog.sina.com.cn/s/blog_6471e1bb01012aql.html)（前后几篇相关都看，有坑的，错误提示也很少，要靠自己想）； 这里还有最新发现的 [打包ANE的视频教程](http://www.tudou.com/plcover/J2Bblm-XIhU/) ！  

> 小记：2014.03.06 项目引用多个ane的话，ios的ane中的类名不能相同，不然编译会冲突，不妨把多个功能些到一个ane里，或者在些单独的ane时候尽量类名长一点