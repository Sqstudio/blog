---
title: 像素级移动（SetPixel）
tags:
  - AS3探究
  - copyPixel
  - setPixel
  - 移动
id: '90'
categories:
  - - AS3探究
date: 2011-01-03 23:54:18
---

常规的建个元件，做段时间轴动画，或者加上一些缓动代码，都可以实现移动的效果，这个是flash的入门知识。 今天要说的是，像素级的运动，通过BitmapData 的 setPixel和copyPixel 方法，改变位图的数据源，实现运动的效果，这个方法在项目实际运用中会大大提高系统的效率。 效果展示：

思路：注册Enter\_Frame事件，每次通过 setPixel 刷新 位图BMP的 数据源BMD，形成动画的效果！

源码如下：

`/* by Nestor/斯樵 sqstudio.com */ package { import flash.display.Bitmap; import flash.display.BitmapData; import flash.display.Sprite; import flash.events.Event; [SWF(width="600",height="300",frameRate="30")] public class setpixel extends Sprite { private var bmd:BitmapData,bm:Bitmap private var id:int=1 public function setpixel() { bmd=new BitmapData(800,600,true,0) bm=new Bitmap(bmd) addChild(bm) addEventListener(Event.ENTER_FRAME,efHandler) } private function efHandler(e:Event):void{ bmd=new BitmapData(800,600,false,0x000000) bm.bitmapData=bmd bmd.lock(); for(var i:int=stage.stageHeight/2-5;i<5+stage.stageHeight/2;i++){ for (var j:int=id;j< 10 +id ;j++){ bmd.setPixel(j,i,0xffffff) } } bmd.unlock(); id +=10; if(id>stage.stageWidth){ id=0; } } } }`

下载地址：[千脑网盘](http://down.qiannao.com/space/file/304149240/-4e0a-4f20-5206-4eab/wp/setpixel-3010-50cf-7d20-7ea7-79fb-52a8-3011.rar/.page) 有问题，欢迎留言讨论！