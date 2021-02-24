---
title: XCODE快速开发ANE步骤和一些常见错误的解决
tags:
  - ane
  - ios
  - XCode
id: '1043'
categories:
  - - 移动探索
date: 2015-05-22 18:47:07
---

使用adobe air制作的移动应用，在对接第三方平台的时候，需要使用ANE来包装本机代码，在对接过程中，我积累了一些打包ANE的经验，记录下来，希望对看到的人有些帮助。 吐槽一下，ANE太难对付了，各种坑，调试也比较麻烦。 1、安装mac版本的AIR SDK 下载地址：[http://www.adobe.com/devnet/air/air-sdk-download-mac.html](http://www.adobe.com/devnet/air/air-sdk-download-mac.html) 2、安装xcode的ANE项目模板(好东西～) 下载地址：[https://github.com/divijkumar/xcode-template-ane](https://github.com/divijkumar/xcode-template-ane) 3、从模板新建项目： ![snap (2).jpg](http://yoyo.play175.com/usr/uploads/2014/09/373566609.jpg) ![snap (3).jpg](http://yoyo.play175.com/usr/uploads/2014/09/3161724819.jpg) 项目建好了的样子： ![snap (4).jpg](http://yoyo.play175.com/usr/uploads/2014/09/2702202715.jpg) 直接点左上角的小三角形编译，报错如下： ![snap (5).jpg](http://yoyo.play175.com/usr/uploads/2014/09/3094537793.jpg) 提示不支持的编译器 解决：我们点击项目属性，选择 LLVM5.13 ![snap (6).jpg](http://yoyo.play175.com/usr/uploads/2014/09/953231852.jpg) 继续点编译： ![snap (7).jpg](http://yoyo.play175.com/usr/uploads/2014/09/3196164692.jpg) 成功了，生成的库文件是libhello.a 接下来我们开始生成ane，点击编译目标： ![snap (8).jpg](http://yoyo.play175.com/usr/uploads/2014/09/5840093.jpg) 选择hello.ane ![snap (9).jpg](http://yoyo.play175.com/usr/uploads/2014/09/1897263116.jpg) 再点小三角编译，成功(在\*.ane上右键选Show in Finder可以定位到这个ane文件)： ![snap (10).jpg](http://yoyo.play175.com/usr/uploads/2014/09/1352824676.jpg) 如果在项目创建页面没有填写这个个： ![snap (11).jpg](http://yoyo.play175.com/usr/uploads/2014/09/2644621939.jpg) 则会提示错误： ![snap (12).jpg](http://yoyo.play175.com/usr/uploads/2014/09/3215069781.jpg) 解决方法是，在项目属性里设置这个宏参数，填上swc文件所在的全路径： ![snap (13).jpg](http://yoyo.play175.com/usr/uploads/2014/09/262000265.jpg) 剩下的事情就是增加你自己需要的接口来，在hello.m文件里，可以看到模板示例方法：isSupported，自己写代码可以参照它。 另外如果需要使用C++和objec混编，则需要项目中至少有一个后缀为mm的源文件，你可以创建一个空mm文件即可。

* * *

在ios本机库，AIR SDK包含了以下系统库： ![snap (14).jpg](http://yoyo.play175.com/usr/uploads/2014/09/1619475565.jpg) 如果项目依赖了除了上面列表之外的库，需要打开项目下的那个：platformoptions.xml 填上依赖的动态库（\*.dylib）或者静态库framework(\*.framework) 动态库用-l前缀，framework用-framework前缀 ![snap (15).jpg](http://yoyo.play175.com/usr/uploads/2014/09/4161221035.jpg) 如果不在上面xml中指定，打包ANE是不会报错的，但是在打包ipa的时候，会报类似下面的错：

```
Undefined symbols for architecture armv7:
  "_crc32", referenced from:
      _PyZlib_crc32 in libpython2.7.a(zlibmodule.o)
  "_inflateEnd", referenced from:
      _Decomp_dealloc in libpython2.7.a(zlibmodule.o)
      _PyZlib_decompress in libpython2.7.a(zlibmodule.o)
      _PyZlib_unflush in libpython2.7.a(zlibmodule.o)
  "_deflateInit_", referenced from:
      _PyZlib_compress in libpython2.7.a(zlibmodule.o)
  "_inflate", referenced from:
      _PyZlib_decompress in libpython2.7.a(zlibmodule.o)
      _PyZlib_objdecompress in libpython2.7.a(zlibmodule.o)
      _PyZlib_unflush in libpython2.7.a(zlibmodule.o)
  "_deflateEnd", referenced from:
```

在使用该ANE打包ipa的过程中，如果使用了多个ANE，且里面引用了同一个公用类，会提示重复的符号： ![snap (1).jpg](http://yoyo.play175.com/usr/uploads/2014/09/3124623426.jpg) 解决方法是在调用adt打包ipa的时候，增加参数：-hideAneLibSymbols yes

* * *

**如果打包ipa的出现错误（找不到\_\_\_divmodsi4符号）：**

```
Packaging: ../hello.ipa
using certificate: hello.p12...

Warning: Resource zh-Hans.lproj has been skipped because of mismatch with suppor
ted languages information in application descriptor.
Undefined symbols for architecture armv7:
  "___divmodsi4", referenced from:
      _absc_solve in D:\\mobile\\wow_mobile\\..\\wow_app\\AOTBuildOutput62399389
92767853027.tmp\\com.q1.haima.o
ld: symbol(s) not found for architecture armv7
Compilation failed while executing : ld64

APK setup creation FAILED.

Troubleshooting:
- did you build your project in FlashDevelop?
- verify AIR SDK target version in bat\app-haima.xml
```

**解决方案：** 1、目标系统版本需要高一些，比如5.1或者6.0以上，在platformoptions.xml 文件的linkerOptions指定参数：-ios\_version\_min，如下：

```
<platform xmlns="http://ns.adobe.com/air/extension/3.1">
    <sdkVersion>5.0</sdkVersion>
    <description > An optional description</description>
    <copyright>2012 (optional)</copyright>
    <linkerOptions>
        <option>-ios_version_min 5.1</option>
        <option>-framework AdSupport</option>
        <option>-framework AudioToolbox</option>
        <option>-lstdc++.6</option>
    </linkerOptions>
</platform>
```

2、如果改sdk版本还不行，则使用下面方案，在.h头文件里增加下面的函数：

```
unsigned long ___udivmodsi4(unsigned long num, unsigned long den, int modwanted)
{
    if (modwanted)
        return num % den;
    else
        return num / den;
}
```

**另外，引起APP闪退的可能原因：** 1、调用API函数的参数类型不正确，objc的参数为NSString类型，但是传int类型，在编译时也不会报错，但是在手机运行时会崩溃！！！需要仔细检查是否有该问题 作者：YoYo，原文地址：[http://yoyo.play175.com/p/xcode-ane.html](http://yoyo.play175.com/p/xcode-ane.html)