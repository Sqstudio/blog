---
title: AS3批量定义变量
tags:
  - AS3定义变量
  - 定义变量
  - 批量定义
id: '306'
categories:
  - - AS3探究
date: 2011-03-15 12:46:34
---

网友问我的一个小问题，问题是：怎么样一次定义100个变量，比如a1，a2，a3....。以前也没怎么注意，就特意想了下，大概想了三种方法，其实也大同小异。 第一种用数组（当然用dictionary也可以）： \[codesyntax lang="actionscript3"\]

var tmpArr:Array = new Array();
 for(var i:int=0;i<100;i++){
 tmpArr\["a"+i\]=1;
 }
 trace(tmpArr\["a"+50\])//输出：1;

\[/codesyntax\]   第二种 用 object：   \[codesyntax lang="actionscript3"\]

 var tmpObj:Object = {};
 for(var i:int=1;i<=100;i++){
tmpObj\["a"+i\]=1;
 }
 trace(tmpObj\["a"+50\])//1;

\[/codesyntax\] 第三中 用动态类： \[codesyntax lang="actionscript3"\]

package
{
public class sam
{
public function sam()
{
var s:samm = new samm();
for(var i:int=0;i<100;i++){
s\["a"+i\]=1;
}
trace(s\["a"+50\])//输出：1;

}
}
}
dynamic class  samm{

}

\[/codesyntax\]   应该还有别的方法，有思路的朋友 不吝赐教！