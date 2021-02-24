---
title: 关于Starling丢失纹理的半天折腾
tags: []
id: '939'
categories:
  - - 移动探索
date: 2014-02-18 15:06:56
---

在手机上调试自己开发的游戏时候，突然收到一条短信，结果造成了Starling的上下文丢失了，已经开启了Starling.handleLostContext = true; 结果恢复纹理的时候报错！！！ [![QQ截图20140218145830](http://qxu2059920095.my3w.com/blog/wp-content/uploads/2014/02/QQ截图20140218145830.jpg)](http://qxu2059920095.my3w.com/blog/wp-content/uploads/2014/02/QQ截图20140218145830.jpg)各种思考： 1.加载方式有问题？（没有用自带AssetManager，是自己写的加载） 2.atf版本有问题？（atf 有两个版本，老版本的Straling不能加载最新的atf格式） 3.没有更新到Starling 1.4.1版本？ ..... 各种测试，各种折腾，最后终于发现： 原来是这样的：游戏里有个loading 转圈圈的，常见的那种 小白点组成的，在创建的时候是通过draw sprite得到的 bitmapData，创建完loading的时候，直接把小bitmapData.dispose()了,结果在恢复纹理的时候，找不到这一小块 bmd，所以报了“#2015 Invalid BitmapData”，所以报错了； o(︶︿︶)o 唉，说多都是泪啊，教训深刻，中午都没休息！ 大意不得！！！ 总结：

> 虽然启用了Starling.handleLostContext = true;但是在游戏内通过代码创建的bmd要格外注意，这些个独立与加载系统之外的小杂毛，往往翻船到这阴沟里！