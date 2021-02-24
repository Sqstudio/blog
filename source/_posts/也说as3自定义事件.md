---
title: 也说AS3自定义事件
tags:
  - 事件
id: '127'
categories:
  - - AS3探究
date: 2011-01-12 23:35:21
---

看到网上有很多关于AS3自定义事件的文章，写的或简洁到不知所云，或复杂到云山雾罩，大多属于懂的人不用看，不懂的人看不懂。不禁想起前端时间刚接触自定义事件时的纠结，看了几篇相关的文章后，也明白作者的意思，结合自己工作中的一些想法，于是就也写篇文章说说自己的理解，凑凑热闹！ 自我感觉自定义事件并不太难，主要还是得理解自定义事件的意义，明白自定义事件有什么用处，懂得自定义事件用在什么地方。 下面就结合我最近做一个小游戏时用到自定义事件的情况介绍下用法。 思路：通过抛出自定义事件，改变当前游戏状态！ 由MainView来管理GameStartView、GamingView和GameEndView三个游戏状态，每个状态有一个按钮，点击后，抛出自定义事件，MainView侦听到事件后，根据参数改变游戏状态；自定义事件类MyGameEvent继承于Event，含有一个nextStatus的属性。 UML图：

[![](http://qxu2059920095.my3w.com/blog/wp-content/uploads/2011/01/GameEventUML.jpg "GameEventUML")](http://qxu2059920095.my3w.com/blog/wp-content/uploads/2011/01/GameEventUML.jpg)

代码方面列出最关键的类：MyGameEvent类。 \[codesyntax lang="actionscript3" container="pre"\]

package com.nestor.event
{
import flash.events.Event;

public class MyGameEvent extends Event
{
// 默认事件类型
public static const Status\_Change:String = "MyGameEvent.Status\_Change";
// 下一游戏状态；
public var nextStatus:String;
public function MyGameEvent(type:String=Status\_Change, bubbles:Boolean=false, cancelable:Boolean=false)
{
super(type, bubbles, cancelable);
}
public function clone():MyGameEvent {
return new MyGameEvent (type, bubbles, cancelable);
}
}
}

\[/codesyntax\]     源码下载： \[download id="3"\]