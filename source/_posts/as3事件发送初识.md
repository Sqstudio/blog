---
title: AS3事件机制初识
tags:
  - AS3探究
  - 事件
  - 发送
  - 机制
id: '70'
categories:
  - - AS3探究
date: 2010-11-29 20:43:43
---

昨天在做一个小游戏的时候，发现事件机制很不错，特地写一写，分享一下！ 我当时是把游戏时间控制单独写了一个time类，游戏开始时60秒倒计时启动，可是时间为0时，怎么通知Main类呢？ 以前也看过事件发送dispatchEvent，不过没怎么理解，不知道怎么用，后来就忘了，现在拾起来再一看，果然很有用，怪不得黑羽连发几篇文章，名为“AS3强大的事件机制”。 dispatchEvent就是可以把time的信息通过事件发送通知给Main类，发送的时候是time类里发送，接受的时候是Main类里的time类实例在侦听接收，所以从头到尾还是一个对象在完成，感觉很合理。 接下来贴效果：

说明：time类的倒计时为0时，会发送事件给Main类，Main类里的time类实例收到信息后，执行下一步操作！

代码如下：

time类：

`package { import flash.display.Sprite; import flash.utils.Timer; import flash.events.TimerEvent; import flash.events.Event; public class Time extends Sprite { public var timer:Timer,tim:int; public function Time() { init(); } private function init() { timer = new Timer(1000); tim = 5;//总时间 timer.start() tim_txt.text = String(tim); trace(tim_txt.text) timer.addEventListener(TimerEvent.TIMER,timeHandler); private function timeHandler(e:TimerEvent) { tim_txt.text=String(tim) if (tim == 0) { timer.stop(); //时间为0时，发送给Main主文档这一事件，让Main进行下一步处理 this.dispatchEvent(new TimerEvent(TimerEvent.TIMER_COMPLETE)); } else { tim--; } } } }`

Main类： `package { import flash.display.Sprite; import flash.events.*; public class Main extends Sprite { private var time_mc:Time; public function Main() { init(); } private function init() { time_mc=new Time(); addChild(time_mc); time_mc.addEventListener(TimerEvent.TIMER_COMPLETE,timeCom); } public function timeCom(e:TimerEvent ) { timcom_txt.text = "Main类收到time类所发送的信息，时间到，Main类可以进行下一步处理"; } } }` 源码下载：[千脑网盘](http://down.qiannao.com/space/file/304149240/-4e0a-4f20-5206-4eab/wp/AS3-4e8b-4ef6-673a-5236-521d-59cb.rar/.page) 上面写的属于事件机制里很简单的部分，随后在小游戏里我还用了自定义事件，但是理解的还不深，下次发文章时再写！