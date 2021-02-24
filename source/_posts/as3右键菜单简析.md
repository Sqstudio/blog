---
title: AS3右键菜单简析
tags:
  - ContextMenu
id: '365'
categories:
  - - AS3探究
date: 2011-04-24 00:00:32
---

Flash里的按钮(Button)、影片剪辑(MoiveClip)和文本(TextFiled)都有右键菜单的属性ContentMenu，我们可以新建一个ContextMenu变量来改变右键菜单，然后每个对每个菜单项ContextMenuItem侦听其选中事件（MENU\_ITEM\_SELECT），来进行响应的操作。 [](http://blog.sqstudio.com/wp-content/uploads/2011/04/rightBtn.jpg)[](http://blog.sqstudio.com/wp-content/uploads/2011/04/rightBtn.jpg)[](http://blog.sqstudio.com/wp-content/uploads/2011/04/rightBtn.jpg)[![](http://qxu2059920095.my3w.com/blog/wp-content/uploads/2011/04/RightBtn.jpg "RightBtn")](http://qxu2059920095.my3w.com/blog/wp-content/uploads/2011/04/RightBtn.jpg) 这篇文章只是右键菜单的基本应用知识，比较简单，更加详尽完善的右键菜单可以参照这几个类ContextMenu、ContextMenuBuiltInItems、ContextMenuClipboardItems、ContextMenuEvent和ContextMenuItem。灵活运用这些类的方法属性就可以做出自己需要的右键菜单了。

在上面的swf上点击右键 即可看到效果

局部重点代码： \[codesyntax lang="actionscript3"\]

private var myContextMenu:ContextMenu;
private var menuLabel:String = "斯樵工坊(SQStudio.com)";
private var label:TextField;

public function RightClick() {
myContextMenu = new ContextMenu();
myContextMenu.hideBuiltInItems();
addCustomMenuItems();
this.contextMenu = myContextMenu;
}
/\*\*
 \*增加菜单项
 \*
 \*/
private function addCustomMenuItems():void {
//ContextMenuItem 的参数说明这个看下api文档，第一个参数是名字，第二个是这个菜单项上面要不要显示一个分隔线，
//   第三个参数 就是是否可用，不可用则为灰色  ，第四个参数是否可见

var item1:ContextMenuItem = new ContextMenuItem(menuLabel);
myContextMenu.customItems.push(item1);
item1.addEventListener(ContextMenuEvent.MENU\_ITEM\_SELECT, menuItem1SelectHandler);

var item2:ContextMenuItem = new ContextMenuItem("QQ聊天");
myContextMenu.customItems.push(item2);
item2.addEventListener(ContextMenuEvent.MENU\_ITEM\_SELECT, menuItem2SelectHandler);

var item3:ContextMenuItem = new ContextMenuItem("by 斯樵/Nestor",true,false);
myContextMenu.customItems.push(item3);

}
/\*\*
 \*菜单项1的响应事件，即跳转到我博客斯樵工坊
 \* @param e
 \*
 \*/
private function menuItem1SelectHandler(e:ContextMenuEvent):void{
var rqs:URLRequest = new URLRequest("http://www.sqstudio.com");
navigateToURL(rqs,"\_blank");
}
/\*\*
 \*菜单项1的响应事件，即和我用QQ即时聊天，如果我在线的话
 \* @param e
 \*
 \*/
private function menuItem2SelectHandler(e:ContextMenuEvent):void{
//\*\*\*   url我做了修改 ，即123456789，你可以改成自己的QQ号
var rqs:URLRequest = new URLRequest("http://wpa.qq.com/msgrd?v=3&uin=123456789&site=qq&menu=yes");
navigateToURL(rqs,"\_blank");
}

\[/codesyntax\] 源码下载： \[download id="10" format="3"\]