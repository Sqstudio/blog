---
title: Bitmap残影练习
tags:
  - Bitmap
  - 残影
id: '1030'
categories:
  - - AS3探究
date: 2014-07-11 16:36:18
---

有点空，写下效果练习下。 效果截图： [![shadow](http://qxu2059920095.my3w.com/blog/wp-content/uploads/2014/07/shadow.png)](http://qxu2059920095.my3w.com/blog/wp-content/uploads/2014/07/shadow.png)     \[codesyntax lang="actionscript3"\]

package com.shadow
{
import flash.display.Bitmap;
import flash.display.BitmapData;
import flash.display.Sprite;
import flash.display.StageAlign;
import flash.display.StageScaleMode;
import flash.events.Event;
import flash.geom.ColorTransform;
import flash.geom.Matrix;
import flash.geom.Rectangle;

\[SWF(width="800",height="600")\]
public class Shadow extends Sprite
{

\[Embed(source="../../../assets/phone.png")\]
private var Phone:Class;

private var bgbmd:BitmapData;
private var phone:Bitmap;

private const WIDTH:int = 800;
private const HEIGHT:int = 600;
private var speedX:int;
private var speedY:int;

public function Shadow()
{
super();
this.addEventListener(Event.ADDED\_TO\_STAGE,addToStage);
}

protected function addToStage(event:Event):void
{

stage.align = StageAlign.TOP\_LEFT;
stage.scaleMode = StageScaleMode.NO\_SCALE;

phone = new Phone() as Bitmap;
bgbmd = new BitmapData(WIDTH,HEIGHT);
var bg:Bitmap = new Bitmap(bgbmd);
addChild(bg);

this.speedX = 20;
this.speedY = 10;

this.addEventListener(Event.ENTER\_FRAME,loop);
}

protected function loop(event:Event):void
{

phone.x += speedX;
phone.y += speedY;
if(phone.x > WIDTH  phone.x < 0) speedX\*=-1;
if(phone.y > HEIGHT  phone.y < 0) speedY\*=-1;

bgbmd.draw(phone,new Matrix(1,0,0,1,phone.x,phone.y));
bgbmd.colorTransform(new Rectangle(0,0,WIDTH,HEIGHT),new ColorTransform(1,1,1,0.8));

}
}
}

\[/codesyntax\]