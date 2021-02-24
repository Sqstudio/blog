---
title: AIR最小化系统托盘
tags:
  - Air
  - 系统托盘
id: '420'
categories:
  - - AS3探究
date: 2011-06-25 22:48:35
---

这是一个简单AIR应用HelloWorld，功能描述如下： 1.窗体上可以通过最小化，正常化和最大化，并退出系统。 2.在窗体上按下鼠标可以拖动。 3.能够生成系统的托盘图标，托盘图标目前设置了两个菜单项目，一个是指向本站，还有一个是退出系统。 运行效果如下： [![](http://qxu2059920095.my3w.com/blog/wp-content/uploads/2011/05/AIR最小化系统托盘.jpg "AIR最小化系统托盘")](http://qxu2059920095.my3w.com/blog/wp-content/uploads/2011/05/AIR最小化系统托盘.jpg)     安装包如下：\[download id="12"\]，可以下载该exe安装包，安装好显示运行效果。   部分重点源码如下： \[codesyntax lang="actionscript3"\]

public function createIcon():void
{
NativeApplication.nativeApplication.autoExit = false;

var iconMenu:NativeMenu = new NativeMenu();
var urlCommand:NativeMenuItem = iconMenu.addItem(new NativeMenuItem("sqstudio.com"));
urlCommand.addEventListener(Event.SELECT, function(event:Event):void
{
navigateToURL(new URLRequest("http://www.sqstudio.com"));
}
);
var exitCommand:NativeMenuItem = iconMenu.addItem(new NativeMenuItem("退出系统"));
exitCommand.addEventListener(Event.SELECT, function(event:Event):void
{
NativeApplication.nativeApplication.icon.bitmaps = \[\];
NativeApplication.nativeApplication.exit();
}
);

if (NativeApplication.supportsSystemTrayIcon)
{
NativeApplication.nativeApplication.autoExit = false;
var icon:Loader = new Loader();
icon.contentLoaderInfo.addEventListener(Event.COMPLETE, function(e:Event):void{
NativeApplication.nativeApplication.icon.bitmaps=\[e.target.content.bitmapData\];

});
icon.load(new URLRequest("/ico/tubiao1.png"));
var systray:SystemTrayIcon = NativeApplication.nativeApplication.icon as SystemTrayIcon;
systray.tooltip = "AIR-by SQStudio.com";
systray.menu = iconMenu;
systray.addEventListener(MouseEvent.CLICK,geneCKHandler);
}
}

\[/codesyntax\]   源码下载：\[download id="13"\]  （用Flash CS5 打开）