---
title: 获取Obj变量地图
tags: []
id: '763'
categories:
  - - AS3探究
date: 2012-10-15 20:53:08
---

获取Obj上的所有public属性，可以用于两个对象的属性的快速匹配，详情请见 flash.utils.describeType 方法的API \[codesyntax lang="actionscript3"\]

public function initParams(obj:Object):void{
this.paramVo = new ParamsVo();
var obj:Object = getVariablesMapObj(paramVo);
for(var i:String in obj){
trace(i);
if(obj.hasOwnProperty(i)){
paramVo\[i\] = obj\[i\];
}
}
}

/\*\*
 \* 获得变量地图
 \* @param target
 \* @return
 \*
 \*/
private function getVariablesMapObj(target:Object):Object
{
var obj:Object = new Object();
var dt:XML=describeType(target);
for each(var node:XML in dt.variable)
{
obj\[node.@name\] = node.@type;
}
return obj;
}

\[/codesyntax\]