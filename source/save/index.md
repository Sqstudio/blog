---
title: 点滴收藏
id: '477'
tags: []
categories:
  - - uncategorized
date: 2011-09-27 10:39:50
---

> 1\. 在Textfiled中使用<br>标签时，要设置其multiline = true！ 关于限制输入实例： \[codesyntax lang="actionscript3"\]
> 
> // 示例 1：只允许大写字母 A-Z、空格和数字 0-9。
> my\_ti.restrict = "A-Z 0-9";
> 
> // 示例 2：除小写字母 a-z 外都允许。
> my\_ti.restrict = "^a-z";
> 
> // 示例 3：只允许数字 0-9、连字符 (-)、^ 和 \\
> my\_ti.restrict = "0-9\\\\-\\\\^\\\\\\\\";
> 
> \[/codesyntax\]

 

> 2.几个IP查询接口（JS）： （注意： 这三个接口中，只有新浪接口下有crossdomain.xml文件，所以如果用AS3直接访问的话，建议选择新浪接口，不然会遇到安全沙箱哦！）
> 
>  腾讯的IP地址API接口地址：http://fw.qq.com/ipaddress
>  新浪的IP地址查询接口：http://int.dpool.sina.com.cn/iplookup/iplookup.php?format=js（还有json和xml格式）
>  新浪多地域测试方法：http://int.dpool.sina.com.cn/iplookup/iplookup.php?format=js&ip=12.130.132.30
>  搜狐IP地址查询接口（默认GBK）：http://pv.sohu.com/cityjson
>  搜狐IP地址查询接口（可设置编码）：http://pv.sohu.com/cityjson?ie=utf-8
>  搜狐另外的IP地址查询接口：http://txt.go.sohu.com/ip/soip
> 
> 这里提供了一个绕开 crossdomain.xml的方法：[猥琐的绕开AS3的crossdomain.xml权限](http://www.sqstudio.com/as3/583.html)

 

> 3.在Flash builder 4.5中创建手机项目，调试时候显示空白，只需禁用或者删除火狐的firebug就可以了 FB4.5创建手机项目启动报空对象错误，删除火狐Flashbug即可 总之，FB上的一些诡异问题可能和Firefox上的调试插件有关，常用的调试插件有Firebug、Flashbug和FlashFireBug等

 

> 4.SVN插件地址：http://subclipse.tigris.org/update\_1.6.x SVN版本从1.6升级到1.7：此次升级 TortoiseSVN和subclipse版本不兼容。如果安装的是TortoiseSVN1.6版本的，可以用之前直接在安装新软件 输入 http://subclipse.tigris.org/update\_1.6.x。这样来安装subclipse的1.6版，如果使用的是 TortoiseSVN1.7版本的，要么在 安装新软件输入 http://subclipse.tigris.org/update\_1.8.x， svn1.8安装后，出现“Unable to load default SVN client Subversion”可安装SVN中的 JavaHL Native Library Adpter解决，虽然标记的是非必须  
> 
>          格式化插件（flexFormatter）： http://flexformatter.googlecode.com/svn/trunk/FlexFormatter/FlexPrettyPrintCommandUpdateSite

 

> 5\. google code SVN，只读项目地址是http://...... ，此状态不可提交 ,显示405错误， 有提交权限的地址是 https://....

 

> 6\. 给URLRequest的url为""时，报Error #2000错误
> 
>  var loader:Loader = new Loader();
>  loader.load(new URLRequest(""));
>  //报错：SecurityError: Error #2000: 没有活动的安全上下文。

 

> 7\. for in 和for each in 的区别: for in 遍历的是键，可以用来遍历object，for each in 遍历的是值，可以遍历Array；

   

> 8.Event的stopPropagation和stopImmediatePropagation的区别 stopPropagation 停止后续节点的事件侦听，当前节点注册的侦听器可用（如对同一个对象的同一个事件类型注册多个侦听）。后续则不可用。 stopImmediatePropagation 停止当前节点和后续节点的事件侦听，有优先级，则按优先级顺序，无优先级则按注册侦听器的顺序。即执行到发出stopImmediatePropagation命令的侦听器后面的侦听器不可用。

 

> 9.非AS3相关：定时关机 运行cmd： ①at 10:00 shutdown -s 在十点关机 ②at 显示任务 ③at 1/delete 删除已设任务1、2、3····· Ⅰ)shutdown -s -t 3600 三千六百秒（一小时）后关机 Ⅱ)shutdown -a 取消关机任务

 

> 10 AS3的 with写法： \[codesyntax lang="actionscript3"\]
> 
>       private function withFun():void{
>             with (this.graphics)
>             {
>                 beginFill(0xffffff);
>                 drawRect(0, 0, 100, 100);
>                 endFill();
>             }
>         }
> 
> \[/codesyntax\]

> 11.AS3加密：[activetuts+ 挺不错](http://active.tutsplus.com/tutorials/workflow/protect-your-flash-files-from-decompilers-by-using-encryption/ "AS3加密")

 

> 12.火狐插件地址 Flash player :http://www.adobe.com/support/flashplayer/downloads.html Firebug： https://addons.mozilla.org/en-US/firefox/addon/firebug/ Flashbug：https://addons.mozilla.org/en-US/firefox/addon/flashbug/

 

> 13.XML节点读取写法xml\['节点名'\] \[codesyntax lang="actionscript3"\]
> 
> var xml:XML=<root>
> <i1>1</i1>
> <i2>2</i2>
> <i3>3</i3>
> </root>
> 
> for(var i:int=0;i<3;i++){
> trace(String(xml\["i"+(i+1)\]));
> }
> //输出 1 2 3
> 
> \[/codesyntax\]

 

> 14.关闭页面提示
> 
> window.onbeforeunload=functions (){
> 
>             return  "确定退出吗？";
> }

 

> 15.阻止手机back键返回： 以NativeApplication.nativeApplication侦听KEY\_DOWN即可，侦听KEY\_UP无效，事件触发太晚了
> 
>       NativeApplication.nativeApplication.addEventListener(KeyboardEvent.KEY\_DOWN,exitHandler);  
>       private function exitHandler(e:KeyboardEvent):void
>         {
>             if(e.keyCode == Keyboard.BACK){
>                 e.preventDefault();
>                 trace("确定退出吗？");
>             }
>         }

 

> 16.MiniComponent  示例：http://www.bit-101.com/MinimalDesigner/ http://www.minimalcomps.com/oldsite/

 

> 17.FP11内置JSON 比第三方json快三倍：stringify--encode  parse--decode； 需要用到官方最新的swc类库支持：（将playerglobal.swc替换sdk里的相同文件即可，除了内置JSON外，还有许多其他的新增api） [![Download](http://www.adobe.com/images/icons/download.gif)Download the playerglobal.swc to target the 11.2 APIs](http://fpdownload.macromedia.com/get/flashplayer/updaters/11/playerglobal11_2.swc) (.swc, .329KB)

 

> 18.SWF直接获取html的get参数 如网址为  http://www.sqstudio.com/sample.html?a=1&b=2 之前为了获取问号后的参数，需要借助于JS，下面这种方法 可以直接在SWF里获取get参数 \[codesyntax lang="php"\]
> 
> if(ExternalInterface.available){
> var param:String = ExternalInterface.call("document.location.search.toString");
> }else{
> trace("ExternalInterface disable!");
> }
> 
> \[/codesyntax\] 此时获取到的 param 为  “？a=1$b=2”

 

> 19.JS iframe传递参数
> 
>  <body>
>     <iframe id="sample" width="100%" height="100%" scrolling="no" frameborder="0" ></iframe>
>     <script type="text/javascript">
>         document.getElementById("sample").src ="http://sqstudio.com"+location.search ;
>     </script>
> </body>

 

> 20.JSP页面 JS中调用java变量
> 
> <%@ page language="java" pageEncoding="UTF-8"%>
> <%
>     String id = request.getParameter("xn\_sig\_user");
> %>
> <body>
>   <script type="text/javascript">
>     var a = '<%=id %>';
>     alert(a);
>   </script>
> </body>

 

> 19.JS iframe传递参数
> 
>  <body>
>     <iframe id="sample" width="100%" height="100%" scrolling="no" frameborder="0" ></iframe>
>     <script type="text/javascript">
>         document.getElementById("sample").src ="http://sqstudio.com"+location.search ;
>     </script>
> </body>

 

> 21.Stage3D：
> 
> 显卡驱动：2009.1.1之前
> 测试：http://wonderfl.net/c/jJzk
> 官方要求：http://helpx.adobe.com/x-productkb/multi/stage3d-unsupported-chipsets-drivers-flash.html

 

> 22.代码里没有用到的类 想把它编译进去可以用\[Frame(factoryClass="SystemManager")\]元标签

 

> 23.常用字体 黑体：SimHei 宋体：SimSun 新宋体：NSimSun 仿宋：FangSong 楷体：KaiTi 仿宋\_GB2312：FangSong\_GB2312 楷体\_GB2312：KaiTi\_GB2312 微软雅黑体：Microsoft YaHei

 

> 24.获取类 \[codesyntax lang="actionscript3"\]
> 
> getDefinitionByName('com.sqstudio.demo::Main') as Class;
> 
> \[/codesyntax\] 注：需要将类引入，仅仅import还是不够的，需要 创建实例，类才会被真正编译到swf，此时使用getDefinitionByName方法才有效，否则会报#1065错：
> 
>  ReferenceError: Error #1065: 变量 Main 未定义。
>  at global/flash.utils::getDefinitionByName()

> 25.关于策略文件crossdomain.xml不在根目录：
> 
> <cross-domain-policy>
> 
> <allow-access-from domain="\*"/>
> 
> </cross-domain-policy>
> 
> Flash player 10 安全策略发生变化
> 
> <cross-domain-policy>
>     <site-control permitted-cross-domain-policies="all"/>
>  </cross-domain-policy>

> 26.修改FlashBuilder的注释做着@author： Step1：打开FlashBuiler安装目录中的FlashBuilder.ini（如D:\\Program Files\\Adobe\\Adobe Flash Builder 4\\‍FlashBuilder.ini）。 Step2：在文件的最后加入一行：-Duser.name=你的名字

> 27.用URLLoader+URLRequset上传图片数据可能需要修改urlRequest的contentType： contentType默认的是application/x-www-form-urlencoded 如果改为contentType="application/octet-stream";还不行的话，可以尝试改为image/png ,image/jpg

> 28。AS3直接发送邮件： http://code.google.com/p/smtpmailer/ http://www.bytearray.org/?p=27

> 29.json插件 FB:http://sourceforge.net/projects/eclipsejsonedit/ 需要建json文件才能编辑 Notepad++：http://sourceforge.net/projects/nppjsonviewer/?source=recommended

> 30.goagent google code: http://code.google.com/p/goagent/ 使用方式: http://www.x-berry.com/goagent（google 服务） http://ishare.cn.ms/archives/552 （php代理）

> 31.Array 互转 Vector array to vect：  var vect:Vectot.<\*> = Vectot.<\*>(arr); vect to array:     var arr:Array = vect.join(",").split(",");

> 32.flash翻书特效 http://pageflip.hu/pageflip2/index.php

> 33.分享到QQ空间和朋友 http://sns.qzone.qq.com/cgi-bin/qzshare/cgi\_qzshare\_onekey?url=http://sqstudio.com 其他参数： \[codesyntax lang="javascript"\]
> 
> <script type="text/javascript">// <!\[CDATA\[
> (function(){
> var p = {
> url:location.href,
> showcount:'1',/\*是否显示分享总数,显示：'1'，不显示：'0' \*/
> desc:'',/\*默认分享理由(可选)\*/
> summary:'',/\*分享摘要(可选)\*/
> title:'',/\*分享标题(可选)\*/
> site:'',/\*分享来源 如：腾讯网(可选)\*/
> pics:'', /\*分享图片的路径(可选)\*/
> style:'203',
> width:98,
> height:22
> };
> var s = \[\];
> for(var i in p){
> s.push(i + '=' + encodeURIComponent(p\[i\]''));
> }
> document.write(\['<a version="1.0" class="qzOpenerDiv" href="http://sns.qzone.qq.com/cgi-bin/qzshare/cgi\_qzshare\_onekey?',s.join('&'),'" target="\_blank">分享</a>'\].join(''));
> })();
> // \]\]></script>
> <script charset="utf-8" type="text/javascript" src="http://qzonestyle.gtimg.cn/qzone/app/qzlike/qzopensl.js#jsdate=20111201"></script>
> 
> \[/codesyntax\]

> 34.AS3移动端广告【ADMob】 ios verion：[http://lilili87222.github.com/admob-for-flash/](http://lilili87222.github.com/admob-for-flash/http://) android version [http://code.google.com/p/flash-for-mobile/](http://code.google.com/p/flash-for-mobile/)

> 34.SWF编码解码相关 [http://zero202.sinaapp.com](http://zero202.sinaapp.com/)

> 35.头像编辑 [http://www.faceyourmanga.com/editmangatar.php](http://www.faceyourmanga.com/editmangatar.php)

> 36.素材网站 1>iconfinder.com 2>freepik.com 3>findicons.com

> 37.数组排序 arr.sortOn(\["quality","level"\],\[Array.NUMERIC Array.DESCENDING, Array.NUMERIC Array.DESCENDING\]); 分别对quality按照数组\[0\],对level按照数组\[1\]的规则进行排序，可以自定义每个属性的排序规则 arr.sortOn(\["quality","level"\],Array.NUMERIC Array.DESCENDING); 对"quality"和"level"都按照Array.NUMERIC Array.DESCENDING进行排序

 

> 38.各种寻路 [http://qiao.github.io/PathFinding.js/visual/](http://qiao.github.io/PathFinding.js/visual/)

 

> 39.获取北京时间 http://www.time.ac.cn/timeflash.asp?user=flash

 

> 40.手机归属地接口 [http://webservice.webxml.com.cn/WebServices/MobileCodeWS.asmx](http://webservice.webxml.com.cn/WebServices/MobileCodeWS.asmx)

 

> 41.Flash Builder的未知错误处理 a. 使用FB打包IOS程序（.ipa文件）时出现“打包应用程序时出错：map failed”。原因是内存不足，把没用的程序都给关掉，重启一下FB。如果还是出现同样的问题，说明你的程序资源包太大，需要升级内存了 b.启动FB时弹框提示“Failed to create the Java Virtual Machine”。解决办法：用文件搜索找到FB安装目录下的FlashBuilder.ini、FlashBuilderc.ini和eclipse.ini，把这些文件里面的“-XX:MaxPermSize=256”改成“-XX:MaxPermSize=128”，大功告成  

   

> 42. 知识点普及 此ADT非彼ADT AIR中所说的ADT 指的是  AIR Developer Tool google Android中的ADT 指的是 Android Develop Tool

 

> 43.AIR的发布说明 [http://helpx.adobe.com/air/air-releasenotes.html](http://helpx.adobe.com/air/air-releasenotes.html "AIR发布说明")

 

> 44.Flash Builder 4.7 已知问题 http://helpx.adobe.com/cn/flash-builder/release-note/flash-builder-4-7-known.html （安装SourceMate插件时中招，坑！）

 

> 45.FLEX编译器参数中-SWF-VERSION与-TARGET-PLAYER之关系 http://zengrong.net/post/1486.htm  
> 
> Flash Player
> 
> AIR
> 
> Flex
> 
> \-swf-version
> 
> \-target-player
> 
> 发布日期
> 
> 9
> 
> 3
> 
> 9
> 
> 9
> 
> 10.0
> 
> 1.5
> 
> 4.0
> 
> 10
> 
> 10.0.0
> 
> 10.1
> 
> 2.0/2.5
> 
> 4.1
> 
> 10
> 
> 10.1.0
> 
> 10.2
> 
> 2.6
> 
> 4.5/4.5.1
> 
> 11
> 
> 10.2.0
> 
> 2011-2-9
> 
> 10.3
> 
> 2.7
> 
> 12
> 
> 10.3.0
> 
> 11.0
> 
> 3.0
> 
> 13
> 
> 11.0.0
> 
> 2011-10-4
> 
> 11.1
> 
> 3.1
> 
> 4.6
> 
> 14
> 
> 11.1
> 
> 2011-11-7
> 
> 11.2
> 
> 3.2
> 
> 15
> 
> 11.2
> 
> 2012-3-28
> 
> 11.3
> 
> 3.3
> 
> 16
> 
> 11.3
> 
> 2012-6-8
> 
> 11.4
> 
> 3.4
> 
> Adobe Flex 4.6/Apache Flex 4.8
> 
> 17
> 
> 11.4
> 
> 2012-08-21
> 
> 11.5
> 
> 3.5
> 
> Adobe Flex 4.6/Apache Flex 4.8
> 
> 18
> 
> 11.5
> 
> 2012-11-06
> 
> 11.6
> 
> 3.6
> 
> Adobe Flex 4.7/Apache Flex 4.9
> 
> 19
> 
> 11.6
> 
> 2013-02-12
> 
> 11.7
> 
> 3.7
> 
> Adobe Flex 4.7/Apache Flex 4.9
> 
> 20
> 
> 11.7
> 
> 2013-04-09
> 
> 11.8
> 
> 3.8
> 
> Adobe Flex 4.7/Apache Flex 4.10
> 
> 21
> 
> 11.8
> 
> 2013-07-09
> 
> 11.9
> 
> 3.9
> 
> Adobe Flex 4.7/Apache Flex 4.11
> 
> 22
> 
> 11.9
> 
> 2013-10-08
> 
> 12.0
> 
> 4.0
> 
> Adobe Flex 4.7/Apache Flex 4.11
> 
> 23
> 
> 12.0
> 
> 2014-01-14
> 
> 13.0
> 
> 13.0
> 
> Adobe Flex 4.7/Apache Flex 4.12
> 
> 24
> 
> 13.0
> 
> 2014-04-08
> 
>  

 

> 46.优化压缩png [https://tinypng.com/](https://tinypng.com/)

 

> 47.捕获AS3中未处理的全局错误
> 
> \[codesyntax lang="actionscript3"\]
> 
> loaderInfo.uncaughtErrorEvents.addEventListener(UncaughtErrorEvent.UNCAUGHT\_ERROR, onUncaughtError);
> private function onUncaughtError(e:UncaughtErrorEvent):void
> {
>     // Do something with your error.
> }
> 
> \[/codesyntax\]
> 
>  

  48.