---
title: 在Mac上配置adb命令
tags:
  - adb
  - Android
  - mac
id: '1093'
categories:
  - - Android开发
date: 2016-01-07 20:28:23
---

在Mac上配置adb命令 在Mac OS中使用adb命令时，应进行变量配置，步骤如下：

一、终端中输入 cd ~

二、输入touch .bash\_profile 回车

touch:如果没有，则创建文件，如果有，更新一下文件时间

三、输入open -e .bash\_profile

open:打开文件 回车后会在TextEdit中打开这个文件（如果未配置过环境变量，应该是个空白文件）。在文件中加如以下内容

> export PATH=${PATH}:/Users/xxx/Library/Android/sdk/platform-tools

上面是我的电脑上的路径，请自行修改，其实就是你android sdk的路径