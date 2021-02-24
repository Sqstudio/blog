---
title: starling 粒子系统学习(Particle-System)
tags:
  - Particle
  - starling
  - 粒子系统
id: '783'
categories:
  - - AS3探究
date: 2012-12-19 15:31:32
---

学习Starling的粒子效果，做了个关于火柴燃烧效果的小应用。 [![火柴效果](http://qxu2059920095.my3w.com/blog/wp-content/uploads/2012/12/unnamed-file.jpg)](http://qxu2059920095.my3w.com/blog/wp-content/uploads/2012/12/unnamed-file.jpg) 主要代码如下： \[codesyntax lang="actionscript3"\]

package
{
import flash.display.Sprite;
import flash.display.StageAlign;
import flash.display.StageScaleMode;

import starling.core.Starling;
\[SWF(width="480", height="800", frameRate="60", backgroundColor="0x333333")\]
public class ParticlesTest extends Sprite
{
public function ParticlesTest()
{
stage.align = StageAlign.TOP\_LEFT;
stage.scaleMode = StageScaleMode.NO\_SCALE;

var myStarling:Starling = new Starling(Game,stage);
myStarling.showStats = true;
myStarling.start();
}
}
}

\[/codesyntax\] \[codesyntax lang="actionscript3"\]

package
{
import starling.core.Starling;
import starling.display.Image;
import starling.display.Sprite;
import starling.events.Event;
import starling.events.TouchEvent;
import starling.extensions.PDParticleSystem;
import starling.extensions.ParticleSystem;
import starling.textures.Texture;

public class Game extends Sprite
{
\[Embed(source="../assets/match\_fire.pex", mimeType="application/octet-stream")\]
private static const MatchFireConfig:Class;

\[Embed(source="../assets/match\_fire.png")\]
private static const FirePng:Class;

\[Embed(source="../assets/match\_begin.png")\]
private static const MatchBeginPng:Class;
\[Embed(source="../assets/match\_ing.png")\]
private static const MatchIngPng:Class;
\[Embed(source="../assets/match\_end.png")\]
private static const MatchEndPng:Class;

private var particleSystem:ParticleSystem;
private var matchBegin:Image;
private var matchIng:Image;
private var matchEnd:Image;
private var status:String;
private const STATUS\_BEGIN:String = "BEGIN";
private const STATUS\_ING:String = "ING";
private const STATUS\_END:String = "END";

public function Game()
{
addEventListener(Event.ADDED\_TO\_STAGE,addToStage);
}

private function addToStage(e:Event):void
{
this.matchBegin = new Image(Texture.fromBitmap(new MatchBeginPng()));
this.matchIng = new Image(Texture.fromBitmap(new MatchIngPng()));
this.matchEnd = new Image(Texture.fromBitmap(new MatchEndPng()));
addChild(matchBegin);
addChild(matchIng);
addChild(matchEnd);

var config:XML = XML(new MatchFireConfig());
var texture:Texture = Texture.fromBitmap(new FirePng());
this.particleSystem = new PDParticleSystem(config,texture);
particleSystem.stop();
addChild(particleSystem);
particleSystem.emitterX = 98;
particleSystem.emitterY = 140;
particleSystem.alpha = 0.5;
particleSystem.scaleX = particleSystem.scaleY = 2.5;
Starling.juggler.add(particleSystem);

addEventListener(TouchEvent.TOUCH,touchHandler);
this.status = STATUS\_BEGIN;
updateStatus();
}

private function touchHandler(e:TouchEvent):void
{
if(status == STATUS\_BEGIN){
status = STATUS\_ING;
}else if(status ==STATUS\_ING){
status = STATUS\_END;
}else if(status ==STATUS\_END){
status = STATUS\_BEGIN;
}
updateStatus();
}

private function updateStatus():void{
this.matchBegin.visible = status ==STATUS\_BEGIN;
this.matchIng.visible = status ==STATUS\_ING;
this.matchEnd.visible = status ==STATUS\_END;

if(status == STATUS\_ING){
particleSystem.start();
}else{
particleSystem.pause();
}
}
}
}

\[/codesyntax\]   发布了一个apk，有Andriod的朋友可以试试，设定的60帧/s，最低会掉到30帧/s，没优化.（手机是htc g12的，480x800 如果手机屏幕不一样的话 可能坐标略有差别） 下载(apk): \[download id="14" format="2"\] 项目源码下载：\[download id="15"\]  （环境： Flash Builder 4.7  两个类库(starling和particle system)没有打包，可以在下面的地址下载下） 相关学习资料： starling源码：https://github.com/PrimaryFeather/Starling-Framework 粒子系统：https://github.com/PrimaryFeather/Starling-Extension-Particle-System 粒子系统在线编辑器：http://onebyonedesign.com/flash/particleeditor/