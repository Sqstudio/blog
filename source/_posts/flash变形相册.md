---
title: Flash变形相册
tags:
  - AS3探究
  - flash
id: '41'
categories:
  - - 站长拙作
date: 2010-11-26 22:54:40
---

看到网上很多相册，或者是flash网站的展示页面，有下面这种flash加载下一张图片时的变形效果，感觉很不错，就尝试写了下这个效果。 只是一个很简单 的效果，有兴趣的朋友可扩展下做成相册，呵呵！（有人做的话，做好了记得分享下，我也围观下！） 效果如下：

[![变形相册截图](http://qxu2059920095.my3w.com/blog/wp-content/uploads/2010/11/changephoto.jpg "changephoto")](http://www.sqstudio.com/wp-content/uploads/exer/changephoto/changephoto.swf)

注：1.点击图片  或[此处](http://www.sqstudio.com/wp-content/uploads/exer/changephoto/changephoto.swf) 查看效果， 下载载本地效果更佳！

代码如下： `code: import com.greensock.TweenLite; import com.greensock.TweenLite; import flash.events.MouseEvent; import flash.display.Loader; import flash.net.URLRequest; import flash.events.Event; import flash.display.Sprite; import flash.events.ProgressEvent; /* by Nestor/斯樵 www.sqstudio.com */ var sp:Sprite =new Sprite(); var picLoader:Loader=new Loader(); var picArr = ["image/1.jpg","image/2.jpg","image/3.jpg","image/4.jpg"]; stage.addEventListener(MouseEvent.CLICK,onCK); var id:int = 0; init(); function onCK(e:MouseEvent):void { sp.removeChild(picLoader); picLoader.unload(); init(); stage.removeEventListener(MouseEvent.CLICK,onCK); load_txt.visible = true; } function picPro(e:ProgressEvent):void { load_txt.text = Math.round(e.bytesLoaded / e.bytesTotal * 100).toString() + "%"; } function picCom(e:Event) { load_txt.visible = false; var wid:Number = Number(e.target.width) + 10; var hei:Number = Number(e.target.height) + 10; picLoader.contentLoaderInfo.removeEventListener(Event.COMPLETE,picCom); TweenLite.to(sam_mc,1,{width:wid,height:hei,onComplete:tweenCom}); } function tweenCom() { sp.addChild(picLoader); sp.x = sam_mc.x - sam_mc.width / 2 + 5; sp.y = sam_mc.y - sam_mc.height / 2 + 5; addChild(sp); stage.addEventListener(MouseEvent.CLICK,onCK); } function init() { picLoader.load(new URLRequest(picArr[id])); picLoader.contentLoaderInfo.addEventListener(ProgressEvent.PROGRESS,picPro); picLoader.contentLoaderInfo.addEventListener(Event.COMPLETE,picCom); if (id == picArr.length - 1) { id = 0; } else { id++; } }` 源码下载：[千脑网盘](http://down.qiannao.com/space/file/304149240/-4e0a-4f20-5206-4eab/wp/-76f8-518c-53d8-5f62(www.sqstudio.com).rar/.page) 解压密码：www.sqstudio.com 本实例用所用到的TweenLite类 可到[官网下载](http://www.greensock.com/)！