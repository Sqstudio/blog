---
title: 位图九宫格设置Scale9Grid
tags:
  - Scale9Grid
id: '436'
categories:
  - - AS3探究
date: 2011-08-05 23:00:39
---

[![](http://qxu2059920095.my3w.com/blog/wp-content/uploads/2011/08/scale9Grid.jpg "scale9Grid")](http://qxu2059920095.my3w.com/blog/wp-content/uploads/2011/08/scale9Grid.jpg) 众所周知，位图放大会失真，AS3中的Scale9Grid属性会降低这种不好的影响。对于元件中的矢量图运用九宫格功能非常简单，只需要在元件属性中勾选该选项即可，而对于外部加载的位图来说，应用九宫格来降低放大失真的马赛克影响，就比较麻烦了，笔者也是在网上找的工具类，并进行了加工，来解决这个问题，特与大家共享。以下图片分别是对同一张图片在是否运用九宫格的情况下放大两倍的效果，直接上图，效果对比： [![](http://blog.sqstudio.com/wp-content/uploads/2011/08/scale9%E5%AF%B9%E6%AF%94.jpg "scale9对比")](http://blog.sqstudio.com/wp-content/uploads/2011/08/scale9%E5%AF%B9%E6%AF%94.jpg) 用法： \[codesyntax lang="actionscript3"\]

package
{
import flash.display.Bitmap;
import flash.display.DisplayObject;
import flash.display.Loader;
import flash.display.LoaderInfo;
import flash.display.Sprite;
import flash.events.Event;
import flash.geom.Rectangle;
import flash.net.URLRequest;

public class Scale9 extends Sprite
{
public function Scale9()
{
var loader:Loader = new Loader();
loader.contentLoaderInfo.addEventListener(Event.COMPLETE,comHandler);
loader.load(new URLRequest("img/scale9.png"));
}
private function comHandler(e:Event):void{
var bitmap:Bitmap = LoaderInfo(e.currentTarget).content as Bitmap;
var dobj:DisplayObject = addChild(new Scale9GridDisplayObject(bitmap,new Rectangle(30,10,60,20)));
dobj.x = 100;
dobj.y = 20;
dobj.width \*=2;
dobj.height \*=2;

var oldBmp:Bitmap = new Bitmap(bitmap.bitmapData.clone());
dobj = addChild(oldBmp);
dobj.x = 100;
dobj.y = 170;
dobj.width \*=2;
dobj.height \*=2;
}
}
}

\[/codesyntax\] 工具类 \[codesyntax lang="actionscript3"\]

package
{
import flash.display.Bitmap;
import flash.display.BitmapData;
import flash.display.DisplayObject;
import flash.display.PixelSnapping;
import flash.display.Sprite;
import flash.geom.Point;
import flash.geom.Rectangle;

public class Scale9GridDisplayObject extends Sprite
{
private var \_sourceData:BitmapData;
private var rect:Rectangle;

private var grid00:DisplayObject;
private var grid10:DisplayObject;
private var grid20:DisplayObject;
private var grid01:DisplayObject;
private var grid11:DisplayObject;
private var grid21:DisplayObject;
private var grid02:DisplayObject;
private var grid12:DisplayObject;
private var grid22:DisplayObject;

private var \_width:Number;
private var \_height:Number;

private var \_minWidth:Number;
private var \_minHeight:Number;

public function Scale9GridDisplayObject(source:Object,rectangle:Rectangle = null)
{
if(source is BitmapData){
this.\_sourceData = BitmapData(source);
}else{
this.\_sourceData = new BitmapData(source.width+0.99999,source.height+0.99999,true,0);
this.\_sourceData.draw(visualize(source));
}

if(rectangle != null){
this.rect = rectangle;
}else if(null != source.scale9Grid){
this.rect = source.scale9Grid;
}else{
this.rect = new Rectangle(0,0,\_sourceData.width,\_sourceData.height);
}

this.\_width = \_sourceData.width;
this.\_height = \_sourceData.height;

grid00 = getBitmap(0,0,rect.left,rect.top);
grid01 = getBitmap(rect.left,0,rect.width,rect.top);
grid02 = getBitmap(rect.right,0,this.\_width - rect.right,rect.top);

grid10 = getBitmap(0,rect.top,rect.left,rect.height);
grid11 = getBitmap(rect.left,rect.top,rect.width,rect.height);
grid12 = getBitmap(rect.right,rect.top,this.\_width - rect.right,rect.height);

grid20 = getBitmap(0,rect.bottom,rect.left,this.\_height - rect.bottom);
grid21 = getBitmap(rect.left,rect.bottom,rect.width,this.\_height - rect.bottom);
grid22 = getBitmap(rect.right,rect.bottom,this.\_width - rect.right,this.\_height - rect.bottom);

this.\_minWidth = grid00.width+grid02.width;
this.\_minHeight = grid00.height + grid20.height;
}

public function getBitmap(x:Number,y:Number,width:Number,height:Number):DisplayObject{
if(width<=0height<=0){
return null
}
var bitmapData:BitmapData = new BitmapData(width,height,true,1);
bitmapData.copyPixels(this.\_sourceData,new Rectangle(x,y,width,height),new Point(0,0));
var bitmap:Bitmap = new Bitmap(bitmapData,PixelSnapping.NEVER);
bitmap.x = x;
bitmap.y = y;
this.addChild(bitmap);
return bitmap;
}

override public function set width(newWidth:Number):void{
if(newWidth>= this.\_minWidth){
update(newWidth - this.\_width,0);
this.\_width = newWidth;
}
}

override public function set height(newHeight:Number):void{
if(newHeight>= this.\_minHeight){
update(0,newHeight - this.\_height);
this.\_height = newHeight;
}
}

public function update(diffW:Number,diffH:Number):void{
if(diffW != 0){
diff(grid01,"width",diffW);
diff(grid11,"width",diffW);
diff(grid21,"width",diffW);
diff(grid02,"x",diffW);
diff(grid12,"x",diffW);
diff(grid22,"x",diffW);
}
if(diffH != 0){
diff(grid10,"height",diffH);
diff(grid11,"height",diffH);
diff(grid12,"height",diffH);
diff(grid20,"y",diffH);
diff(grid21,"y",diffH);
diff(grid22,"y",diffH);
}
}

public function diff(obj:DisplayObject,property:String,diffNum:Number):void{
obj\[property\] += diffNum;
}

public function visualize(src:Object):DisplayObject{
if(src== null) return null;
var target:DisplayObject;
if(src is DisplayObject) target = DisplayObject(src);
return target;
}
}
}

\[/codesyntax\]