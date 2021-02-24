---
title: Android文件读写操作
tags:
  - Android
  - File
id: '1075'
categories:
  - - Android开发
date: 2015-09-25 10:11:38
---

Assets：该目录只可读，不可写。使用getResource().getAssets().open("file\_name")获得FileInputStream，进行读取操作 Raw：该目录和Assets一样，可读不可写。使用getResource().openRawResource(R.raw.file\_name)获得FileInputSteam，进行读取操作。 外部SD：可读可写，外部可见。先用Environment.getExtenalStorageDirectory获得sd卡的目录File，先检测是否可用。如果可用，再file = new File(sd\_dir,filename),进行读写操作。 内部文件：