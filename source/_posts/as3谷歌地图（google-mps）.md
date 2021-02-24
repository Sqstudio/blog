---
title: AS3谷歌地图（Google Mps）
tags: []
id: '486'
categories:
  - - 站长拙作
date: 2011-10-10 17:38:29
---

最近做的一个谷歌地图的练习，效果如下：  

  主类的代码：   \[codesyntax lang="actionscript3" lines\_start="1" tab\_width="10"\]

package com.sqstudio.control
{
import com.google.maps.Color;
import com.google.maps.InfoWindowOptions;
import com.google.maps.Map;
import com.google.maps.MapEvent;
import com.google.maps.controls.MapTypeControl;
import com.google.maps.controls.NavigationControl;
import com.google.maps.controls.OverviewMapControl;
import com.google.maps.controls.ScaleControl;
import com.google.maps.services.ClientGeocoder;
import com.google.maps.services.GeocodingEvent;
import com.google.maps.services.GeocodingResponse;
import com.google.maps.services.Placemark;
import com.sqstudio.common.IPProxy;
import com.sqstudio.common.SysConfig;
import com.sqstudio.vo.IpVo;

import flash.events.Event;
import flash.geom.Point;
import flash.text.TextFormat;

import flashx.textLayout.formats.TextAlign;

public class MapController
{
private var googleMap:Map;
private var geocoder:ClientGeocoder;
private var ipVo:IpVo;
public function MapController()
{
init();
}
private function init():void{
googleMap = new Map();
googleMap.sensor = "false";
googleMap.key = SysConfig.MAP\_API\_KEY;
googleMap.addEventListener(MapEvent.MAP\_READY, mapReadyHandler);
googleMap.setSize(new Point(SysConfig.STAGE.stageWidth, SysConfig.STAGE.stageHeight));

//NavigationControl - ScaleControl  为一组UI
//ZoomControl - PositionControl 为一组UI

googleMap.addControl(new ScaleControl());
googleMap.addControl(new NavigationControl());
//googleMap.addControl(new ZoomControl());
//googleMap.addControl(new PositionControl());
//
googleMap.addControl(new MapTypeControl());//地图类型
googleMap.addControl(new OverviewMapControl());//小型地图-缩略图
SysConfig.STAGE.addChild(googleMap);
googleMap.visible = false;
}
/\*\*
 \*地图准备就绪
 \* @param event
 \*
 \*/
private function mapReadyHandler(event:Event):void
{
geocoder = new ClientGeocoder();
geocoder.addEventListener(GeocodingEvent.GEOCODING\_SUCCESS, geocodingSuccessHandler);
geocoder.addEventListener(GeocodingEvent.GEOCODING\_FAILURE, geocodingFailHandler);
var ipProxy:IPProxy = new IPProxy();
ipProxy.getIp(getIpResult);
}
/\*\*
 \*获得IP地址，进行解析
 \* @param ipVo
 \*
 \*/
private function getIpResult(ipVo:IpVo):void{
this.ipVo = ipVo;
geocoder.geocode(this.ipVo.cip);
}
/\*\*
 \*解析地址成功
 \* @param event
 \*
 \*/
private function geocodingSuccessHandler(event:GeocodingEvent):void
{
googleMap.visible = true;
var place:Placemark = GeocodingResponse(event.response).placemarks\[0\];
googleMap.setCenter(place.point,9);//10-缩放级别

var infoWindowOptions:InfoWindowOptions = new InfoWindowOptions();
infoWindowOptions.title = "欢迎访问斯樵工坊(SQStudio.com)";
var titleFmt:TextFormat = new TextFormat(null,12,Color.BLUE);
titleFmt.align = TextAlign.CENTER;
infoWindowOptions.titleFormat = titleFmt;
infoWindowOptions.content = "你的位置："+place.address;
infoWindowOptions.content += "\\n经纬度："+place.point;
infoWindowOptions.width = 260;
infoWindowOptions.hasShadow = true;

googleMap.openInfoWindow(place.point,infoWindowOptions);
}
/\*\*
 \*解析地址失败
 \* @param event
 \*
 \*/
protected function geocodingFailHandler(event:GeocodingEvent):void
{
if(!this.ipVo.isIpError){
trace("解析IP失败："+event.name);
this.ipVo.isIpError = true;
geocoder.geocode(String(this.ipVo.cname));
}else if(!this.ipVo.isNameError){
trace("解析城市名称失败："+event.name);
this.ipVo.isNameError = true;
geocoder.geocode("北京市");//解析失败默认北京
}
}
}
}

\[/codesyntax\] 谷歌Map 中文AS3 API地址： http://code.google.com/intl/zh-CN/apis/maps/documentation/flash/reference.html 获得本地IP方式：

> 2.几个IP查询接口（JS）： （注意： 这三个接口中，只有新浪接口下有crossdomain.xml文件，所以如果用AS3直接访问的话，建议选择新浪接口，不然会遇到安全沙箱哦！）
> 
>  腾讯的IP地址API接口地址：http://fw.qq.com/ipaddress
>  新浪的IP地址查询接口：http://int.dpool.sina.com.cn/iplookup/iplookup.php?format=js（还有json和xml格式）
>  新浪多地域测试方法：http://int.dpool.sina.com.cn/iplookup/iplookup.php?format=js&ip=12.130.132.30
>  搜狐IP地址查询接口（默认GBK）：http://pv.sohu.com/cityjson
>  搜狐IP地址查询接口（可设置编码）：http://pv.sohu.com/cityjson?ie=utf-8
>  搜狐另外的IP地址查询接口：http://txt.go.sohu.com/ip/soip