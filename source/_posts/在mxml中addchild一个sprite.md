---
title: 在mxml中addChild一个sprite
tags: []
id: '758'
categories:
  - - AS3探究
date: 2012-10-04 23:07:51
---

在flex项目中讲一个没有继承UIComponent的flash的DisplayObject加载(addChild)到舞台上 \[codesyntax lang="actionscript3"\]

<?xml version="1.0" encoding="utf-8"?>
<s:Application name="Spark\_SpriteVisualElement\_addChild\_test"
        xmlns:fx="http://ns.adobe.com/mxml/2009"
        xmlns:s="library://ns.adobe.com/flex/spark"
        xmlns:mx="library://ns.adobe.com/flex/halo"
        initialize="init();">

    <fx:Script>
        <!\[CDATA\[
            private spr1:Sprite = new Sprite();

            private function init():void {
                spr1.graphics.beginFill(0xFF0000, 0.5);
                spr1.graphics.drawRect(10, 10, 100, 80);
                spr1.graphics.endFill();
                con.addChild(spr1);
            }
        \]\]>
    </fx:Script>

    <s:SpriteVisualElement id="con" />

</s:Application>

\[/codesyntax\]