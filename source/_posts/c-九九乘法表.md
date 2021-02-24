---
title: C# 九九乘法表
tags: []
id: '1146'
categories:
  - - 周边技术
date: 2016-09-09 17:09:57
---

\[codesyntax lang="php"\]

              for (int i \= 1; i <\= 9; i++) {
 for (int j \= 1; j <\= i; j++) {
 Console.Write ("{1}\*{0}\={2}\\t",i,j,i\*j);
 }
 Console.WriteLine ();
 }

\[/codesyntax\]   [![snip20160909_2](http://qxu2059920095.my3w.com/blog/wp-content/uploads/2016/09/Snip20160909_2.png)](http://qxu2059920095.my3w.com/blog/wp-content/uploads/2016/09/Snip20160909_2.png)