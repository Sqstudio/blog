---
title: google SVN 无法获得上传密码的解决办法
tags: []
id: '603'
categories:
  - - 杂谈天下
date: 2012-02-19 10:20:49
---

问题描述：在使用google SVN时，点击查看项目密码，总是访问不到，链接被重置了 解决方法：修改本地dns设置，添加“74.125.71.94 code.google.com”记录。 1. cmd打开命令行窗口，执行"ping google.com.hk"（注，自google退出中国大陆后，现在的google中文默认的跳转到香港网站即com.hk），得到当前可用的Google ip：74.125.71.94。 2. 找到本地的dns配置文件路径，windows 7 下为“C:\\Windows\\System32\\drivers\\etc\\hosts”将“  74.125.71.94 code.google.com ”添加到最后一行即可（不带引号）。 4. 在命令行窗口执行“ipconfig /flushdns”刷新本地缓存。