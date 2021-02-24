---
title: WordPress提示“在裁剪您的图像时发生了错误”的解决方法
tags: []
id: '1242'
categories:
  - - 杂谈天下
date: 2017-02-14 23:42:58
---

问题：在WordPress中使用裁剪图片功能时，出现:”在裁剪您的图像时发生了错误。”或者”There has been an error cropping your image.” 原因：缺少PHP GD库。 Ubuntu下运行： \[c\]sudo apt-get install php5-gd\[/c\] CentOS下运行： \[csharp\]yum install php-gd\[/csharp\] 安装PHP-GD库后重启服务器即可使用~