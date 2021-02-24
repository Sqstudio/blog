---
title: Unity导出Android项目报错：Unable to list target platforms.
tags: []
id: '1354'
categories:
  - - Android开发
date: 2017-08-16 11:22:28
---

由于sdk升级到26，所以在Unity5.3.6导出Android项目时，报错如下：

> Error building Player: CommandInvokationFailure: Unable to list target platforms. Please make sure the android sdk path is correct. See the Console for more details. /Library/Java/JavaVirtualMachines/jdk1.8.0\_45.jdk/Contents/Home/bin/java -Xmx2048M -Dcom.android.sdkmanager.toolsdir="/Users/nestordu/Library/Android/sdk/tools" -Dfile.encoding=UTF8 -jar "/Applications/Unity5.3.6p2/PlaybackEngines/AndroidPlayer/Tools/sdktools.jar" - stderr\[ Error:Invalid command android \] stdout\[ \]

导出项目时，调用了Android的SDK的tools下的命令，因为从25升级到26变化较大，所以命令不可用了“Invalid command android”。 解决方式如下： 方法1（直接删掉tools，下载25.2.5版本）

```
cd $ANDROID_HOME #切换到android sdk目录下
rm -rf tools #删除tools
wget http://dl.google.com/android/repository/tools_r25.2.5-ma‌​cosx.zip #下载
unzip tools_r25.2.5-macosx.zip #解压
```

方法2（通过sdkmanager的命令来降低版本，此方法未验证）

cd $ANDROID\_HOME/sdk/tools/bin
sdkmanager --uninstall "tool;android-26"
sdkmanager --install "tool;android-25"
命令参考：[https://developer.android.com/studio/command-line/sdkmanager.html](https://developer.android.com/studio/command-line/sdkmanager.html)