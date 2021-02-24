---
title: 贝塞尔曲线学习---QQ部落箭塔弓箭飞行模拟
tags: []
id: '768'
categories:
  - - 公式算法
date: 2012-11-02 21:39:34
---

[![](http://qxu2059920095.my3w.com/blog/wp-content/uploads/2012/11/archerStudy.jpg "archerStudy")](http://qxu2059920095.my3w.com/blog/wp-content/uploads/2012/11/archerStudy.jpg) 本文部分素材来自QQ部落（http://bl.qq.com）,仅作研究学习之用，如有侵权，请与本站联系。 SWF效果如下：  本实例用的是二次贝塞尔曲线，详情见代码。   逻辑类如下： \[codesyntax lang="php"\]

package com.sqstudio.study.qqbl.archer
{
import com.sqstudio.study.util.Bezier;

import flash.display.Sprite;
import flash.display.Stage;
import flash.events.Event;
import flash.geom.Point;
/\*\*
 \*QQ部落箭塔之弓箭飞行
 \* @author Nestor
 \* @website sqstudio.com
 \*
 \*/
public class ArcherStudy extends Sprite
{
private var arrow:UI\_Arrow;
private var drawPathSp:Sprite;

private var startPoint:Point;

private var endPoint:Point;

private var controlPoint:Point;
private var tower:UI\_Tower;
private var monster:UI\_Monster;
private var steps:uint;
private var crtStep:int;
public function ArcherStudy()
{
//塔
tower = new UI\_Tower();
tower.shooterLeft.stop();
tower.shooterRight.stop();
this.addChild(tower);
tower.x = 100;
tower.y = 150;

//怪
monster = new UI\_Monster();
this.addChild(monster);
monster.x = 400;
monster.y = 300;
//箭头
this.arrow = new UI\_Arrow();
this.addChild(arrow);
//初始化数据
startPoint = new Point(tower.x,tower.y-30);
controlPoint = new Point(monster.x-200,tower.y-130);
endPoint = new Point(monster.x,monster.y-20);
steps = Bezier.init(startPoint,controlPoint,endPoint,8);
this.crtStep = 0;//当前步

this.addEventListener(Event.ENTER\_FRAME,loopEfHandler);
//路径绘制SP
drawData();
}
/\*\*
 \*循环帧频函数
 \* @param e
 \*
 \*/
private function loopEfHandler(e:Event):void
{
var tmpArr:Array = Bezier.getAnchorPoint(crtStep);
arrow.x =  tmpArr\[0\];
arrow.y =  tmpArr\[1\];
arrow.rotation =  tmpArr\[2\];
this.crtStep++;
if(crtStep>steps){
crtStep=0;
if(Math.random()>0.5){
tower.shooterLeft.gotoAndPlay(2);
}else{
tower.shooterRight.gotoAndPlay(2);
}
}
}
/\*\*
 \*绘制路线
 \*
 \*/
private function drawData():void{
this.drawPathSp = new Sprite();
this.addChild(drawPathSp);
drawPathSp.graphics.lineStyle(1,0,0.5);
drawPathSp.graphics.moveTo(startPoint.x, startPoint.y);
drawPathSp.graphics.curveTo(controlPoint.x, controlPoint.y,endPoint.x, endPoint.y);
drawPathSp.graphics.endFill();
drawP(startPoint);
drawP(endPoint);
drawP(controlPoint);
drawPathSp.graphics.moveTo(startPoint.x,startPoint.y);
drawPathSp.graphics.lineTo(endPoint.x,endPoint.y);
drawPathSp.graphics.lineTo(controlPoint.x,controlPoint.y);
drawPathSp.graphics.lineTo(startPoint.x,startPoint.y);
}
/\*\*
 \*生成顶点
 \* @param p
 \* @param type
 \* @return
 \*
 \*/
public function drawP(p:Point):Sprite{
var sp:Sprite = new Sprite();
this.addChild(sp);
sp.graphics.beginFill(0xffffff\*Math.random());
sp.graphics.drawCircle(0,0,4);
sp.graphics.endFill();
sp.x = p.x;
sp.y = p.y;
return sp;

}
}
}

\[/codesyntax\]   实例中，怪物的位置是固定，贝塞尔曲线的控制点也是手动调整的，这两个因素在实际游戏中是不一样的，尤其是控制的点，也应该有计算公式，具体的算法就留待你来扩展了，具体表现效果可以到QQ部落游戏中去体验（声明，我不是托儿，QQ部落也是抄袭国外一款塔防游戏的[_Kingdom_ Rush](http://kingdom-rush.com/)） 工具类（来自网上，略作整理）： \[codesyntax lang="php"\]

package com.sqstudio.study.util
{
import flash.geom.Point;

public class Bezier
{

//  =====================================  属性

//  对外变量
private static var p0:Point;// 起点
private static var p1:Point;// 控制点
private static var p2:Point;// 终点
private static var step:uint;// 分割份数

//  辅助变量
private static var ax:int;
private static var ay:int;
private static var bx:int;
private static var by:int;

private static var A:Number;
private static var B:Number;
private static var C:Number;

private static var total\_length:Number;// 长度

//  =====================================  方法

//  速度函数
private static function s (t:Number):Number
{
return Math.sqrt(A \* t \* t + B \* t + C);
}

//  长度函数
private static function L (t:Number):Number
{
var temp1:Number = Math.sqrt(C + t \* (B + A \* t));
var temp2:Number = (2 \* A \* t \* temp1 + B \*(temp1 - Math.sqrt(C)));
var temp3:Number = Math.log(B + 2 \* Math.sqrt(A) \* Math.sqrt(C));
var temp4:Number = Math.log(B + 2 \* A \* t + 2 \* Math.sqrt(A) \* temp1);
var temp5:Number = 2 \* Math.sqrt(A) \* temp2;
var temp6:Number = (B \* B - 4 \* A \* C) \* (temp3 - temp4);

return (temp5 + temp6) / (8 \* Math.pow(A, 1.5));
}

//  长度函数反函数，使用牛顿切线法求解
private static function InvertL (t:Number, l:Number):Number
{
var t1:Number = t;
var t2:Number;
do
{
t2 = t1 - (L(t1) - l)/s(t1);
if (Math.abs(t1-t2) < 0.000001) break;
t1 = t2;
}while(true);
return t2;
}

//  =====================================  封装

//  返回所需总步数
/\*\*
 \*获得所需步数
 \* @param $p0
 \* @param $p1
 \* @param $p2
 \* @param $speed
 \* @return
 \*
 \*/
public static function init ($p0:Point, $p1:Point, $p2:Point, $speed:Number):uint
{
p0   = $p0;
p1   = $p1;
p2   = $p2;
//step = 30;

ax = p0.x - 2 \* p1.x + p2.x;
ay = p0.y - 2 \* p1.y + p2.y;
bx = 2 \* p1.x - 2 \* p0.x;
by = 2 \* p1.y - 2 \* p0.y;

A = 4\*(ax \* ax + ay \* ay);
B = 4\*(ax \* bx + ay \* by);
C = bx \* bx + by \* by;

//  计算长度
total\_length = L(1);

//  计算步数
step = Math.floor(total\_length / $speed);
if (total\_length % $speed > $speed / 2)step ++;

return step;
}

// 根据指定nIndex位置获取锚点：返回坐标和角度
public static function getAnchorPoint (nIndex:Number):Array
{
if (nIndex >= 0 && nIndex <= step)
{
var t:Number = nIndex/step;
//  如果按照线行增长，此时对应的曲线长度
var l:Number = t\*total\_length;
//  根据L函数的反函数，求得l对应的t值
t = InvertL(t, l);

//  根据贝塞尔曲线函数，求得取得此时的x,y坐标
var xx:Number = (1 - t) \* (1 - t) \* p0.x + 2 \* (1 - t) \* t \* p1.x + t \* t \* p2.x;
var yy:Number = (1 - t) \* (1 - t) \* p0.y + 2 \* (1 - t) \* t \* p1.y + t \* t \* p2.y;

//  获取切线
var Q0:Point = new Point((1 - t) \* p0.x + t \* p1.x, (1 - t) \* p0.y + t \* p1.y);
var Q1:Point = new Point((1 - t) \* p1.x + t \* p2.x, (1 - t) \* p1.y + t \* p2.y);

//  计算角度
var dx:Number = Q1.x - Q0.x;
var dy:Number = Q1.y - Q0.y;
var radians:Number = Math.atan2(dy, dx);
var degrees:Number = radians \* 180 / Math.PI;

return new Array(xx, yy, degrees);
}
else
{
return \[\];
}
}
}
}

\[/codesyntax\] 延伸阅读：http://zh.wikipedia.org/zh-cn/%E8%B4%9D%E5%A1%9E%E5%B0%94%E6%9B%B2%E7%BA%BF