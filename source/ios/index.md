---
title: 移动笔记
id: '873'
tags: []
categories:
  - - uncategorized
date: 2013-05-18 10:51:17
---

记录开发ios过程中的点滴，共享给更多想要进行ios开发的ASer！  

> 1. CNAPS Code 现在填写银行信息的时候，需要这个CNAPS Code，中文叫做“银行网点联行号”,12位数字，也可以在这个地址查到：[https://e.czbank.com/CORPORBANK/WebBank?&tranFlag=0&dse\_operationName=wgQueryUnionBankSrvOp](https://e.czbank.com/CORPORBANK/WebBank?&tranFlag=0&dse_operationName=wgQueryUnionBankSrvOp)

 

> 2.应用支持的设备，Device Family : iPhone / iPod Touch, iPad 这个在项目的xml配置里设置，如下，<string>1</string>代表iPhone/iPod，<string>2</string>代表iPad \[codesyntax lang="php"\]
> 
> <array>
> <string>1</string>
> <string>2</string>
> </array>
> 
> \[/codesyntax\]

 

> 3.iPhone5尺寸检测 iPhone5的尺寸是1136x640，但是如果你放的启动图片还是Default@2x.png(960x640),那么ios就认为你的app也是这个尺寸，应用启动后上下留空，居中显示，AS3所检测到的stage.fullScreenHeight=960，解决这个问题，只需要放一张启动图片Default-586.png(1136x640)即可

 

> 4.应用状态识别
> 
> *   Waiting for upload 等待上传，只有这种状态下才可以上传IPA
> *   Waiting for Review 等待审核，上传成功后开始排队等待审核
> *   In Review 正在接受审核
> *   Rejected 拒绝，会显示拒绝原因 参考【5.被拒原因-Apple 官方说明】
> *   Developer Reject 开发者撤回，开发者可能出于某种原因要撤回文件进行修改，在下次上传之前必须进入Binary Details页面修改应用状态为Waiting for upload，才可以进行下次上传。上传后会重新排队。

 

> 5.被拒原因【持续更新】 [App提交审核被拒的原因汇总](http://blog.csdn.net/mad1989/article/details/8308935) Apple 官方说明：[https://developer.apple.com/appstore/resources/approval/guidelines.html](https://developer.apple.com/appstore/resources/approval/guidelines.html)

 

>  6.内支付商品创建流程 [http://blog.devtang.com/blog/2012/12/09/in-app-purchase-check-list/](http://blog.devtang.com/blog/2012/12/09/in-app-purchase-check-list/)

 

> 7.内支付的 验证地址的坑 在我们公司的测试服务器中，我们会连接苹果的测试服务器（https://sandbox.itunes.apple.com/verifyReceipt）验证。 在我们部署在线上的正式服务器中，我们会连接苹果的正式服务器（https://buy.itunes.apple.com/verifyReceipt ）验证。 我们提交给苹果审核的是正式版，我们以为苹果审核时，我们应该连接苹果的线上验证服务器来验证购买凭证。结果我理解错了，苹果在审核App时，只会在sandbox环境购买，其产生的购买凭证，也只能连接苹果的测试验证服务器。但是审核的app又是连接的我们的线上服务器。所以我们这边的服务器无法验证通过IAP购买，造成我们app的又一次审核被拒。 解决方法是判断苹果正式验证服务器的返回code，如果是21007，则再一次连接测试服务器进行验证即可。苹果的这一篇文档上有对返回的code的详细说明。 [http://www.leiphone.com/iap-traps.html](http://www.leiphone.com/iap-traps.html)

 

> 8,ios启动画面设置
> 
> 文件名
> 
> 图像大小
> 
> 用法
> 
> Default.png
> 
> 320 x 480
> 
> iPhone，标准分辨率
> 
> Default@2x.png
> 
> 640 x 960
> 
> iPhone，高分辨率
> 
> Default-568h@2x.png
> 
> 640 x 1136
> 
> iPhone, 高分辨率, 16:9 高宽比
> 
> Default-Portrait.png
> 
> 768 x 1004（AIR 3.3 以及更低版本）768 x 1024（AIR 3.4 以及更高版本）
> 
> iPad，纵向
> 
> Default-Portrait@2x.png
> 
> 1536 x 2008（AIR 3.3 以及更低版本）1536 x 2048（AIR 3.4 以及更高版本）
> 
> iPad，高分辨率，纵向
> 
> Default-PortraitUpsideDown.png
> 
> 768 x 1004（AIR 3.3 以及更低版本）768 x 1024（AIR 3.4 以及更高版本）
> 
> iPad，倒置纵向
> 
> Default-PortraitUpsideDown@2x.png
> 
> 1536 x 2008（AIR 3.3 以及更低版本）1536 x 2048（AIR 3.4 以及更高版本）
> 
> iPad，高分辨率，倒置纵向
> 
> Default-Landscape.png
> 
> 1024 x 768
> 
> iPad，左横向
> 
> Default-LandscapeLeft@2x.png
> 
> 2048 x 1536
> 
> iPad，高分辨率，左横向
> 
> Default-LandscapeRight.png
> 
> 1024 x 768
> 
> iPad，右横向
> 
> Default-LandscapeRight@2x.png
> 
> 2048 x 1536
> 
> iPad，高分辨率，右横向
> 
> Default-example.png
> 
> 320 x 480
> 
> 标准 iPhone 上的 example://URL
> 
> Default-example@2x.png
> 
> 640 x 960
> 
> 高分辨率 iPhone 上的 example:// URL
> 
> Default-example~ipad.png
> 
> 768 x 1004
> 
> 纵向 iPad 上的 example:// URL
> 
> Default-example-Landscape.png
> 
> 1024 x 768
> 
> 横向 iPad 上的 example:// URL
> 
>  

9.获取应用版本号

> \[codesyntax lang="actionscript3"\]
> 
> var descriptor:XML = NativeApplication.nativeApplication.applicationDescriptor;
> var ns:Namespace = descriptor.namespaceDeclarations()\[0\];
> var versionNumber:String = descriptor.ns::versionNumber;
> 
> \[/codesyntax\] 注意XML的命名空间的，这样XML的所有配置信息都可以取到了！

10.AIR 移动设备上的存储控制

> Android
> 
> iOS
> 
> File.applicationDirectory
> 
> 通过 URL 只读（非本机路径）
> 
> 只读
> 
> File.applicationStorageDirectory
> 
> 可用
> 
> 可用
> 
> File.cacheDirectory
> 
> 可用
> 
> 可用
> 
> File.desktopDirectory
> 
> SDCard 的根目录
> 
> 不可用
> 
> File.documentsDirectory
> 
> SDCard 的根目录
> 
> 可用
> 
> File.userDirectory
> 
> SDCard 的根目录
> 
> 不可用
> 
> File.createTempDirectory()
> 
> 可用
> 
> 可用
> 
> File.createTempFile()
> 
> 可用
> 
> 可用
> 
>   延伸阅读：http://www.cnblogs.com/eran/p/3755541.html

 

> 11.手机跳转到某个应用（需应用市场-安卓） navigateToURL(new URLRequest("market://details?id=air."+id));

   

> 12 icon尺寸，平台区分 [http://help.adobe.com/zh\_CN/air/build/WS901d38e593cd1bac1e63e3d129907d2886-8000.html](http://help.adobe.com/zh_CN/air/build/WS901d38e593cd1bac1e63e3d129907d2886-8000.html)

 

> 13\.  测试IOS支付，遇到无效的商品id，以下两种原因会造成： 1.不能是越狱手机 2.设置商品id，可能不会立即生成，最迟24小时 原因还有很多，各种设置不正确等

 

> 14.多个ios的ane 项目引用多个ane的话，ios的ane中的类名不能相同，不然编译会冲突，不妨把多个功能些到一个ane里，或者在些单独的ane时候尽量类名长一点

 

> 15.appstore的应用地址中参数mt和ls的含义 链接地址如https://itunes.apple.com/us/app/xing-ji-fang-kuai/id825376269?ls=1&mt=8 mt 代表 meta-type，有效值如下： 1   Music 2   Podcasts 3   Audiobooks 4   TV Shows 5   Music Videos 6   Movies 7   iPod Games 8   Mobile Software Applications 9   Ringtones 10  iTunes U 11  E-Books 12  Desktop Apps 当链接进行查询时，如果没有定义id，就有可能出现不同类别的内容，但是名字相同，例如某专辑的名字和某个app的名字重合。这时mt就起作用了。 ls代表link special，当查询的类型为某首歌曲时，不定义ls，默认指向歌曲的专辑，定义后直接进入该歌曲并试播。

 

> 16.ios内购测试----测试帐号 测试内购时，先在“通用”设置里把原有帐号退出，然后在itunesconnect.apple.com上添加test user，帐号密码随便填，邮箱不需要是真实的，打开应用，点击购买，提示要登录账户时登录测试帐号即可，注意，不要直接在“通用”或者appstore里登录测试帐号，否则测试帐号会失效，直接在应用中的弹出提示里登录，即可！

 

> 17.MAC10.9安装教程 http://bbs.weiphone.com/read-htm-tid-7625465.html

 

> 18.ane收藏 [ http://www.riaxe.com/blog/200-adobe-air-anes/]( http://www.riaxe.com/blog/200-adobe-air-anes/)

 

> 19.三星输入法问题 三星输入法在按回车时，没有响应Action事件（editText.setOnEditorActionListener(new OnEditorActionListener() ） 加以下代码ok了，还没搞清为什么，在其他输入法下都没问题。 editText.setKeyListener(new TextKeyListener(TextKeyListener.Capitalize.WORDS,true));

 

> 20.ANE开发的官方文档 [http://help.adobe.com/zh\_CN/air/extensions/air\_extensions.pdf](http://help.adobe.com/zh_CN/air/extensions/air_extensions.pdf)

  21.