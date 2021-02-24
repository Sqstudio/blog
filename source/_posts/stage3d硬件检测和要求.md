---
title: Stage3D硬件检测和要求
tags: []
id: '655'
categories:
  - - 杂谈天下
date: 2012-05-26 09:30:52
---

一、官方要求：[http://helpx.adobe.com/x-productkb/multi/stage3d-unsupported-chipsets-drivers-flash.html](http://helpx.adobe.com/x-productkb/multi/stage3d-unsupported-chipsets-drivers-flash.html)

> 所有平台 任何不支持Pixel Shader 2.x及以上的GPU Intel GMAs that DON’T support Pixel Shader 2.x: 740, i752, IEG, IEG2, GMA 900, GMA 950, GMA 3000, GMA 3100, GMA 3150 Chrome VIA Chipset Windows
> 
> 任何早于2009年一月一日的驱动
>  NVIDIA driver 8.17.11.9621
>  NVIDIA driver 8.17.11.8267
>  Any Intel GMA driver before X.X.10.2021
> 
> Macintosh
> 
> All Intel GMAs
> ATI Radeon X1600
>  ATI Radeon 2400
>  Firefox 3.6 and earlier
>  Mac OS X 10.5
> 
>   \*Linux系统仅支持软渲染 \*Mac和Windows上Intel HD GPU完全支持 推荐 因为2009一月1号之前的驱动全部不支持，强烈推荐你升级显卡驱动。如果升级后仍只能使用软渲染，你的显卡很可能不被支持。

二、硬件检测 （1）快速检测：[http://wonderfl.net/c/jJzk](http://wonderfl.net/c/jJzk) （2）另一种检测方式

运行这个工具 [http://zp.amsnet.pl/cdragan/d3dcaps.html](http://zp.amsnet.pl/cdragan/d3dcaps.html) 得到一个叫 ‘devcaps.txt’ 的文件。 查看两个着色器的值。

VERTEX\_SHADER\_VERSION FFFE0200

PIXEL\_SHADER\_VERSION FFFF0200

忽略FFFE/FFFF, 只看红色部分的值，这个值必须都大于0200 才能支持molehill, 否则就软解。 注意，这两个值会在devcaps.txt文件中出现两次，用后面出现的那一组 （第一组的值是0104，忽略即可），如果上述值正常，但让没有效果，那就参考上文 硬件要求，也许你的显卡不支持。