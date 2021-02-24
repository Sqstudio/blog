---
title: AS3中的Inline（内联函数）
tags: []
id: '1026'
categories:
  - - AS3探究
date: 2014-07-08 19:18:00
---

**内联函数**

是将在函数被调用的地方使用替换成该函数实际的代码,这样就少了一次函数调用,也就少了很多地址查找,入栈出栈的操作,对于频繁调用的函数,就会有明显的性能提升。

**Flash Builder 4.7设置**

1、AIR SDK 3.4以上版本才附带ASC 2.0。

2、在“项目属性——ActionScript编译器——附加的编译器参数”中填写“-inline”。

**函数成为内联函数的前提条件**

1、只有标记为 final、static 的函数或全局函数才能嵌入\[Inline\]。

2、函数不能报考任何Activation Object（包括...参数和arguments对象）。

3、函数不能包含任意try catch或width语句。

4、函数不能包含任何函数闭包。

5、函数体不能超过50条语句。

**测试结论**

**1、final或static的getter和setter函数会自动成为内联函数，不需要\[Inline\]元数据，且在未开启“-inline”参数时，已默认为内联函数。**

**2、闭包（function内含function）不能成为内联函数，且自身效率非常低。**

> 延伸阅读：[http://gotoandlearn.com/play.php?id=170](http://gotoandlearn.com/play.php?id=170)（gotoandlearn的视频）