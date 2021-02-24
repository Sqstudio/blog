---
title: 使用多个iOS ANE的duplicate symbol问题
tags: []
id: '1083'
categories:
  - - 移动开发
date: 2015-12-12 17:31:47
---

当使用AIR进行iOS应用开发时，如果用到了多个iOS ANE，有时在打包时会遇到duplicate symbol问题，比如：

ld: duplicate symbol \_ContextInitializer .....

这个问题的原因是由于多个ANE的iOS原生代码中有同名的类或全局变量。解决方法便是统一给它们加上一个前缀，比如公司和产品的名字组合。 如果使用ANE模板项目创建的ANE，会有 ContextInitializer等同名方法，注意修改   其他关于开发ANE需要注意的问题可以参见这篇文章： 原文：[20 tips for creating Air Native Extensions for iOS](http://www.richardlord.net/blog/20-tips-for-creating-air-native-extensions-for-ios) 中文翻译：[20条开发AIR Native Extension的建议](http://jamesli.cn/blog/?p=1438)