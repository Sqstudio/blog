---
title: Air拖拽操作之拖入拖出
tags:
  - Air
  - drag
  - 拖入
  - 拖出
  - 拖拽
id: '380'
categories:
  - - AS3探究
date: 2011-05-05 00:38:02
---

最近在做一款AIR的播放器，在做到播放列表部分，就想到用AIR的拖拽操作，特意关注了一下Air拖拽操作的一些文章。 [![](http://qxu2059920095.my3w.com/blog/wp-content/uploads/2011/05/AirDragIco.jpg "AirDragIco")](http://qxu2059920095.my3w.com/blog/wp-content/uploads/2011/05/AirDragIco.jpg) //---------------------文字描述不擅长，以下借用RIAMeeting的一篇文章的部分描述------------------------ 我们可以把整个拖拽的过程分为三个阶段：启动，拖动和放下。

1.  启动：用户通过按住鼠标按键从组件或组件中的项目进行拖动，启动拖放操作。涉及的事件包括nativeDragStart 和nativeDragComplete。启动方式是调取NativeDragManager.doDrag()，这个方法调取后，会启动拖拽的过程。
2.  拖动：用户在按住鼠标按键的同时，将鼠标光标移至其他组件、应用程序，或移至桌面。涉及的事件包括nativeDragUpdate，nativeDragEnter，nativeDragOver，nativeDragExit。其中nativeDragEnter事件会被我们用于检测数据格式，以决定是否允许接受该数据格式。如果格式合法，我们可以调用NativeDragManager.acceptDragDrop()方法，允许该数据在组件上放下。这个时候屏幕上光标的形状会有所变化。
3.  放下：用户在符合条件的放置目标上释放鼠标。如果是在AIR应用内部放下，涉及的事件包括nativeDragDrop，我们可以捕获这个事件，来获取剪切板中的数据，如果放下的区域在AIR外部，那么将会由操作系统来确定如何处理该操作。

下图描述了NativeDragEvent的各种类型，不同颜色代表这是由不同的对象调度的。 ![](http://www.riameeting.com/files/casedesign/image/adc_articles/clip_image004(1).jpg) AIR还提供了一个ClipboardFormats类，定义了各种不同的剪切板描述格式，参见下图(这是AIR1.5的版本，在AIR2中增加了一个新的格式，稍后叙述)： ![](http://www.riameeting.com/files/casedesign/image/adc_articles/clip_image006(1).jpg) 按照数据的来源和去处，我们可以将拖拽分为两种类别：拖出和拖入。 //---------------------  End    ------------------------ 借用部分AIRMeeting的图片，AIR的拖拽还是侦听好几个事件就可以了如nativeDragEnter，nativeDragOver等，感觉事件还是有点少，不太够用。 下面是我做的一个小实例，不多少，直接上图，上代码！ 实例的效果是 ，拖拽一张jpg或png位图到AIR上，Air会自动显示该图片，同时将桌面上的图片删除至垃圾桶，然后再从AIR上拖出该图片，该图片就会重新生成在桌面上。 局部DragIn类的代码： \[codesyntax lang="actionscript3"\]

private function onDragIn(event:NativeDragEvent):void{
var transferable:Clipboard = event.clipboard;
if(transferable.hasFormat(ClipboardFormats.FILE\_LIST\_FORMAT)){
var files:Array = transferable.getData(ClipboardFormats.FILE\_LIST\_FORMAT) as Array;
for each(var file:File in files){
if(file.extension =="jpg"  file.extension =="png" ){
NativeDragManager.acceptDragDrop(sp);
break;
}
}
} else {
trace("格式不对，仅接受jpg");
}
}
public var file:File;
private function onDrop(event:NativeDragEvent):void{
var dropfiles:Array= event.clipboard.getData(ClipboardFormats.FILE\_LIST\_FORMAT) as Array;
file = dropfiles\[0\];
trace(file.url);
var loader:Loader = new Loader();
loader.contentLoaderInfo.addEventListener(Event.COMPLETE,picComHandler);
loader.load(new URLRequest(file.url));
}
private function picComHandler(e:Event):void{
var img:Bitmap = (e.currentTarget as LoaderInfo).content as Bitmap;
sp.addChild(img);
file.moveToTrash();
removeListener();
DragOut.getInstance().addListener();
}

\[/codesyntax\] 局部DragOut代码： \[codesyntax lang="actionscript3"\]

private function downHandler(e:MouseEvent):void{
var bmp:Bitmap = sp.getChildAt(0) as Bitmap;
var jpg:JPEGEncoder = new JPEGEncoder();
ba = jpg.encode(bmp.bitmapData);
file = DragIn.getInstance().file;
var transferObject:Clipboard = createClipboard(bmp);
NativeDragManager.doDrag(sp, transferObject, bmp.bitmapData, new Point(-mouseX,-mouseY));
sp.addEventListener(NativeDragEvent.NATIVE\_DRAG\_START,startHandler);
sp.addEventListener(NativeDragEvent.NATIVE\_DRAG\_COMPLETE,comHandler);
}
private function startHandler(e:NativeDragEvent):void{
trace("开始拖拽");
}
private var file:File;
/\*\*
 \*拖拽完成
 \* @param e
 \*
 \*/
private function comHandler(e:NativeDragEvent):void{

var fileStream:FileStream = new FileStream();
fileStream.open(file, FileMode.WRITE);
fileStream.writeBytes(ba,0,ba.length);
fileStream.close();

sp.removeChildAt(0);
sp.removeEventListener(MouseEvent.MOUSE\_DOWN,downHandler);
DragIn.getInstance().init();
}
public function createClipboard(image:Bitmap):Clipboard {
var transfer:Clipboard = new Clipboard();
transfer.setData("bitmap", image, true);
transfer.setData(ClipboardFormats.BITMAP\_FORMAT, image.bitmapData, false);
transfer.setData(ClipboardFormats.FILE\_LIST\_FORMAT,new Array(file),false);
return transfer;
}

\[/codesyntax\] 源码下载：\[download id="11" format="1"\] 本次实例的代码写的有点混乱，看不懂的留言哈，重点代码已经列出！这个拖拽的功能应用很广，比如做列表的动态管理，做文件上传等等凡是涉及到文件列表方面的貌似都可以用，期待你的作品哈！ 扩展阅读：[http://www.riameeting.com/node/486/](http://www.riameeting.com/node/486/)