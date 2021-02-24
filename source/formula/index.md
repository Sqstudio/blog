---
title: 公式汇总
id: '729'
tags: []
categories:
  - - uncategorized
date: 2012-08-20 10:34:18
---

1\.   获取从-1到1 形成波浪线          Math.sin(new Date().time \* 0.004); 2. 判断线段是否在矩形内 \[codesyntax lang="php"\]

/\*\* <p>判断线段是否在矩形内 </p>
 \* 先看线段所在直线是否与矩形相交，
 \* 如果不相交则返回false，
 \* 如果相交，
 \* 则看线段的两个点是否在矩形的同一边（即两点的x(y)坐标都比矩形的小x(y)坐标小，或者大）,
 \* 若在同一边则返回false，
 \* 否则就是相交的情况。
 \* @param linePointX1 线段起始点x坐标
 \* @param linePointY1 线段起始点y坐标
 \* @param linePointX2 线段结束点x坐标
 \* @param linePointY2 线段结束点y坐标
 \* @param rectangleLeftTopX 矩形左上点x坐标
 \* @param rectangleLeftTopY 矩形左上点y坐标
 \* @param rectangleRightBottomX 矩形右下点x坐标
 \* @param rectangleRightBottomY 矩形右下点y坐标
 \* @return 是否相交
 \*/
private function isLineIntersectRectangle(linePointX1:Number,
  linePointY1:Number,
  linePointX2:Number,
  linePointY2:Number,
  rectangleLeftTopX:Number,
  rectangleLeftTopY:Number,
  rectangleRightBottomX:Number,
  rectangleRightBottomY:Number):Boolean
{
var  lineHeight:Number = linePointY1 - linePointY2;
var lineWidth:Number = linePointX2 - linePointX1;  // 计算叉乘
var c:Number = linePointX1 \* linePointY2 - linePointX2 \* linePointY1;
if ((lineHeight \* rectangleLeftTopX + lineWidth \* rectangleLeftTopY + c >= 0 && lineHeight \* rectangleRightBottomX + lineWidth \* rectangleRightBottomY + c <= 0)
 (lineHeight \* rectangleLeftTopX + lineWidth \* rectangleLeftTopY + c <= 0 && lineHeight \* rectangleRightBottomX + lineWidth \* rectangleRightBottomY + c >= 0)
 (lineHeight \* rectangleLeftTopX + lineWidth \* rectangleRightBottomY + c >= 0 && lineHeight \* rectangleRightBottomX + lineWidth \* rectangleLeftTopY + c <= 0)
 (lineHeight \* rectangleLeftTopX + lineWidth \* rectangleRightBottomY + c <= 0 && lineHeight \* rectangleRightBottomX + lineWidth \* rectangleLeftTopY + c >= 0))
{

if (rectangleLeftTopX > rectangleRightBottomX) {
var temp:Number = rectangleLeftTopX;
rectangleLeftTopX = rectangleRightBottomX;
rectangleRightBottomX = temp;
}
if (rectangleLeftTopY < rectangleRightBottomY) {
var temp1:Number = rectangleLeftTopY;
rectangleLeftTopY = rectangleRightBottomY;
rectangleRightBottomY = temp1;   }
if ((linePointX1 < rectangleLeftTopX && linePointX2 < rectangleLeftTopX)
 (linePointX1 > rectangleRightBottomX && linePointX2 > rectangleRightBottomX)
 (linePointY1 > rectangleLeftTopY && linePointY2 > rectangleLeftTopY)
 (linePointY1 < rectangleRightBottomY && linePointY2 < rectangleRightBottomY)) {
return false;
} else {
return true;
}
} else {
return false;
}
}

\[/codesyntax\] 3.计算当前点是否在给点点组成的多边形之内 \[codesyntax lang="php"\]

/\*\*
 \* 计算当前点是否在给点点组成的多边形之内
 \* @param aPoint当前点
 \* @param bPolygon多边形顶点
 \* @return
 \*/
public static function pointInPolygon(testPoint:Array, points:Array):Boolean
{
var angleCount:Number = 0;
for (var i:int=0; i < points.length; i++)
{
var x2:Number;
var y2:Number;
if (i != points.length - 1)
{
x2=points\[i + 1\]\[0\];
y2=points\[i + 1\]\[1\];
}
else
{
x2=points\[0\]\[0\];
y2=points\[0\]\[1\];
}
var p1:Point=new Point(points\[i\]\[0\], points\[i\]\[1\]);
var p2:Point=new Point(x2, y2);
if ((testPoint\[0\] == p1.x && testPoint\[1\] == p1.y)  (testPoint\[0\] == p2.x && testPoint\[1\] == p2.y))
{
angleCount=10;
break;
}
angleCount+=getAngle(new Point(testPoint\[0\],testPoint\[1\]), p1, p2);
}
if(Math.round(angleCount) == 360){
return true;
}else{
return false;
}
}

/\*\*
 \* 计算给定三点的组成三角形的夹角
 \* @param mp夹角点
 \* @param pa点1
 \* @param pb点2
 \* @return 角度值
 \*/
public static function getAngle(mp:Point, pa:Point, pb:Point):Number
{
var p1:Point=new Point(pa.x - mp.x, pa.y - mp.y);
var p2:Point=new Point(pb.x - mp.x, pb.y - mp.y);
var cosValue:Number=(p1.x \* p2.x + p1.y \* p2.y) / (Math.sqrt(p1.x \* p1.x + p1.y \* p1.y) \* Math.sqrt(p2.x \* p2.x + p2.y \* p2.y));
return Math.acos(cosValue) / Math.PI \* 180;
}

\[/codesyntax\] 4.获得p点到过p1和p2直线的距离 \[codesyntax lang="php"\]

/\*\*
 \*获得p点到过p1和p2直线的距离
 \* @param p
 \* @param p1
 \* @param p2
 \* @return
 \*
 \*/
public static function getPtoLineDis(p:Point,p1:Point,p2:Point):Number{
var a:Number = (p1.y-p2.y)/(p1.x-p2.x);
var b:Number = p1.y-a\*p1.x;
var d:Number = Math.abs(a\*p.x - p.y + b)/Math.sqrt(a\*a+1);
return d;
}

\[/codesyntax\]   5. 常用滤镜参数：（可用于flash的滤镜，也可以用于Starling） \[codesyntax lang="actionscript3"\]

/\*\*
 \*灰度滤镜
 \*/
public static const GRAY:Vector.<Number> = Vector.<Number>(\[0.299, 0.587, 0.114, 0, 0,
0.299, 0.587, 0.114, 0, 0,
0.299, 0.587, 0.114, 0, 0,
0, 0, 0, 1, 0\]);
/\*\*
 \*暗灰色滤镜
 \*/
public static const DARK\_GRAY:Vector.<Number> = Vector.<Number>(\[0.2, 0.5, 0.1, 0, 0,
0.2, 0.5, 0.1, 0, 0,
0.2, 0.5, 0.1, 0, 0,
0, 0, 0, 1, 0\]);
/\*\*
 \*高亮滤镜
 \*/
public static const LIGHT:Vector.<Number> = Vector.<Number>(\[
1, 0, 0, 0, 40,
0, 1, 0, 0, 40,
0, 0, 1, 0, 40,
0, 0, 0, 1, 0\]);
/\*\*
 \*反色
 \*/
public static const NEGATIVE:Vector.<Number> = Vector.<Number>(\[
-1, 0, 0, 0, 255,
0, -1, 0, 0, 255,
0, 0, -1, 0, 255,
0, 0, 0, 1, 1\]);

\[/codesyntax\]   6文字描边 参数 \[codesyntax lang="php"\]

BlurFilter.createGlow(0x000000,5,1,3);//Stringling
new GlowFilter(0x000000, 1.0, 2.0, 2.0, 10, 1, false, false) //flash tf

\[/codesyntax\] 以上内容，你若发现错误，请留言或与博主联系，非常感谢！