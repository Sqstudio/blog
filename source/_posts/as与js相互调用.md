---
title: AS与JS相互调用
tags: []
id: '558'
categories:
  - - AS3探究
date: 2011-11-11 14:14:10
---

关于AS和JS的调用，网上文章蛮多的，AS调用JS比较简单，但是JS回调AS有时会有问题，主要是考虑到浏览器的兼容性，比如火狐、IE等.. \[codesyntax lang="actionscript3" lines\_start="1" title="AS源码"\]

private function callJS():void{
ExternalInterface.addCallback("callbackQQPay",callBackFromJs);
var obj:Object = {};
obj.id = 1;
ExternalInterface.call("testItem", obj);

}

private function callBackFromJs(obj:Object=null):void{
trace("OK!");
｝

\[/codesyntax\]     \[codesyntax lang="javascript" lines\_start="1" title="JS代码"\]

function testItem(p){
var swfName = "网页中swf的名字"；
        var flash = thisMovie(swfName);
        flash.callbackQQPay('OK');
}
//查找网页中的swf
function thisMovie(movieName) {
       if (window.document\[movieName\])
       {
                     return window.document\[movieName\];
       }
       if (navigator.appName.indexOf("Microsoft")==-1)
       {
                     if (document.embeds && document.embeds\[movieName\])
                    return document.embeds\[movieName\];
       }
       else
       {
                    return document.getElementById(movieName);
       }
}

\[/codesyntax\]