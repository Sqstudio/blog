---
title: QQ聊天框自动发消息
tags: []
id: '1035'
categories:
  - - 周边技术
date: 2014-08-01 21:08:05
---

\[codesyntax lang="php"\]

Set WshShell= WScript.CreateObject("WScript.Shell")
WshShell.AppActivate "聊天框名字"
for i=1 to 50
WScript.Sleep 500
WshShell.SendKeys "^v"
WshShell.SendKeys "%s"
next

\[/codesyntax\] 存为vbs文件，先复制到剪切板，执行程序，会自动发送！可设置次数和间隔等