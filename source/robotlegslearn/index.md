---
title: Robotlegs
id: '629'
tags: []
categories:
  - - uncategorized
date: 2012-04-18 11:01:05
---

![](http://www.robotlegs.org/images/rl-header-logo-310-253.png "Rebotlegs")   Rebotlegs学习汇总： //================  笔记

>  1. 在Command中用到异步回调函数：【detain-release】
> 
> override public function execute():void
> {
>     commandMap.detain(this);
>     loadingService.load(parseLoadedXML);
> }                       
> override public function parseLoadedXML(dataLoaded:XML):void
> {
>     var updateVO:UserData = parser.parseData(dataLoaded);
>     dispatch(new UserEvent(UserEvent.USER\_LOADED, updateVO));
>     commandMap.release(this);
> }

 

>  2. 在Context中对contextView进行映射-----contextView为App 的一个实例
> 
>         override public function startup():void
>         {
>             injector.mapValue(App,contextView);
>         }

    //=============== 资料 官方  最佳实践：[https://github.com/robotlegs/robotlegs-documentation/blob/master/best-practices-zh-cn.textile](https://github.com/robotlegs/robotlegs-documentation/blob/master/best-practices-zh-cn.textile "https://github.com/robotlegs/robotlegs-documentation/blob/master/best-practices-zh-cn.textile") 天地会Rebotlegs专题：[http://wiki.9ria.com/Robotlegs](http://wiki.9ria.com/Robotlegs "http://wiki.9ria.com/Robotlegs") Robotlegs 框架学习 入门小结: [http://www.cc-space.com/?p=1472](http://www.cc-space.com/?p=1472 "http://www.cc-space.com/?p=1472") 再识RobotLegs–如何去写你的Command: [http://yuyuef.me/secondsightrobotlegs-2/](http://yuyuef.me/secondsightrobotlegs-2/ "http://yuyuef.me/secondsightrobotlegs-2/") [![](http://qxu2059920095.my3w.com/blog/wp-content/uploads/2012/04/robotCompare.png "robotCompare")](http://qxu2059920095.my3w.com/blog/wp-content/uploads/2012/04/robotCompare.png)