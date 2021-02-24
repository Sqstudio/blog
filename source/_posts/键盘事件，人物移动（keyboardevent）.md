---
title: 键盘事件
tags:
  - keyboard
id: '136'
categories:
  - - AS3探究
date: 2011-01-13 19:53:54
---

在我们做游戏时，常常会用到用键盘事件来控制人物移动的情况，如果直接用（KeyboardEvent.KEY\_DOWN）和（KeyboardEvent.KEY\_UP）的话，往往会出现这样或那样的问题，不知道大家都是怎么解决的？  用键盘事件来控制人物移动，最常见的问题是：  1.如果用户按住一个键不释放，如何处理？比如长按向右键的话，会一直运行右键触发的函数！  2.如果用户同时按住多个方向键，如何处理？比如同时按住向右和向上，在前进过程中跳跃；用户还可能中间松开其中一个键等！  这些问题不解决好的话，有可能会造成电脑CPU飙升，严重拖慢系统运行速度，swf 的画面会变得非常卡。  我的思路是 当键盘按下的时候触发一个变量，可能首先大家会想到用布尔值（Boolean），按下为true，释放为false，没错，最初我也用的是布尔值，当用户长按某个键不松时，只是让系统在刷一个布尔值，这样会大大降低程序的运行成本。但是后来我发现用Int的0和1来表示按下和释放两种状态效果更佳，因为0和1这两个数值的本身还可以做为人物移动的坐标系数，这样右进一步简化代码！  最近在学Flash Builder，所以这次的案例是在Flash Builder下写的，用的时Flex3.5的框架，写的时候尽量运行一些架构的思想，就是尽量把视图层、数据层和控制层分开，特意又画了下UML图给大家贴出来！  \[caption id="attachment\_138" align="aligncenter" width="600" caption="键盘事件的UML图"\][![键盘事件的UML图](http://qxu2059920095.my3w.com/blog/wp-content/uploads/2011/01/MoveKey.jpg "MoveKey")](http://qxu2059920095.my3w.com/blog/wp-content/uploads/2011/01/MoveKey.jpg)\[/caption\] 图比较简单，这次主要说的是键盘事件，这些东西顺带说一下，MoveMain是主类，NPCView是视图类，KeyLogic是进行逻辑判断的控制类，GameModel是数据类，　NPCVO声明了一些变量。  下面开始贴案例效果： 

 

说明：先用鼠标在下面的swf上点下，获取焦点，然后用上下左右方向键控制npc的移动！ 

接下来是代码： 

//  代码看不懂的地方可以跳过，只要看懂其中的大意，明白思路就好了！ 

MoveMain（主类）： 

`<?xml version="1.0" encoding="utf-8"?> <mx:Application xmlns:mx="http://www.adobe.com/2006/mxml" layout="absolute" width="600" height="400" applicationComplete="application1_applicationCompleteHandler(event)" xmlns:vo="com.nestor.vo.*" xmlns:view="com.nestor.view.*" > <mx:Script> <![CDATA[ import com.nestor.ctrl.KeyLogic; import com.nestor.model.GameModel; import com.nestor.vo.NPCVO; import mx.events.FlexEvent; private var keyLogic:KeyLogic=new KeyLogic()； private var playerVo:NPCVO=GameModel.getGameModel().playerVO; protected function application1_applicationCompleteHandler(event:FlexEvent):void { stage.addEventListener(KeyboardEvent.KEY_DOWN,keyLogic.downHandler); stage.addEventListener(KeyboardEvent.KEY_UP,keyLogic.upHandler); var timer:Timer=new Timer(30); timer.addEventListener(TimerEvent.TIMER,tHandler) timer.start(); } private function tHandler(e:TimerEvent):void{ var spdX:int=5; var spdY:int=5; Npc.x += (playerVo.isLeft * (-spdX) + playerVo.isRight * spdX); Npc.y += (playerVo.isUp * (-spdY) + playerVo.isDown * spdY); } ]]> </mx:Script> <view:NPCView id="Npc" x="100" y="100"/> </mx:Application>`  

NPCView类：（视图类） `<?xml version="1.0" encoding="utf-8"?> <mx:Canvas xmlns:mx="http://www.adobe.com/2006/mxml" width="100" height="100"> <mx:Button  label="NPC" width="100" height="100" fontSize="32" fontFamily="Georgia"/> </mx:Canvas>`  KeyLogic类（逻辑控制类）  `package com.nestor.ctrl { /* * @ Nestor/斯樵 */ import com.nestor.model.GameModel; import com.nestor.vo.NPCVO; import flash.events.KeyboardEvent; import flash.ui.Keyboard; public class KeyLogic { private var playerVo:NPCVO=GameModel.getGameModel().playerVO; public function KeyLogic() { } public function downHandler(e:KeyboardEvent):void{ switch (e.keyCode){ case Keyboard.UP: playerVo.isUp=1; break; case Keyboard.DOWN: playerVo.isDown=1; break; case Keyboard.LEFT: playerVo.isLeft=1; break; case Keyboard.RIGHT: playerVo.isRight=1; break; } } public function upHandler(e:KeyboardEvent):void{ switch (e.keyCode){ case Keyboard.UP: playerVo.isUp=0; break; case Keyboard.DOWN: playerVo.isDown=0; break; case Keyboard.LEFT: playerVo.isLeft=0; break; case Keyboard.RIGHT: playerVo.isRight=0; break; } } } }`  GameModel类（数据类）  `package com.nestor.model { import com.nestor.vo.NPCVO; public class GameModel { public var playerVO:NPCVO; public var CrtLabel:String; private static var instance:GameModel=null; public function GameModel() { playerVO=new NPCVO(); } public static function getGameModel():GameModel{ return instance = new GameModel(); } } }`   NPCVO类（声明Model中数据）  `package com.nestor.vo { public class NPCVO { public var isLeft:int=0; public var isRight:int=0; public var isUp:int=0; public var isDown:int=0; public function NPCVO() { } } }` 源码下载： \[download id="2"\]