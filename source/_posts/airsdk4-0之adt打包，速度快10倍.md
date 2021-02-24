---
title: AIRSDK4.0之ADT打包IPA，速度快10倍
tags:
  - adt打包
  - ipa
  - useLegacyAOT
id: '936'
categories:
  - - 移动探索
date: 2014-01-24 16:02:42
---

更新了最新的AIR SDK 4.0，加入参数-useLegacyAOT no 打包ipa，果然快了很多，官方说的是快10倍，貌似差不多！   \[codesyntax lang="actionscript3"\]

@echo off
::
::   quickly package ipa
::    by nestor 2014.01.21
::
rem ------------------------
::change path（切换到当前目录下）
cd /d "%~dp0"
cd /d "%cd%\\"
::cd /d ..

echo "%~dp0"

rem ------------ set adt variable（设置adt变量） ----------------------------
::key（证书）
set KEY\_PATH="D:\\keystore\\wen\\dev.p12"
set KEY\_PASS=111111
set KEY\_MOBILE\_PROVISION="D:\\keystore\\wen\\dev.mobileprovision"
::all files（所有文件路径设置）
set ROOT\_PATH=

set FILE\_SWF=%ROOT\_PATH%sh.swf
set FILE\_XML=%ROOT\_PATH%sh-app.xml
set FILE\_LOGO=%ROOT\_PATH%bgLogo.png
set FILE\_IPHONE4\_DEFAULT=%ROOT\_PATH%Default.png
set FILE\_IPHONE5\_DEFAULT=%ROOT\_PATH%Default-568h@2x.png
set FILE\_RES=%ROOT\_PATH%res
::target ipa name（ipa包的名字）
set TARGET\_NAME=sh%date:~5,2%%date:~8,2%.ipa

rem ------------package（打包）-----------------------------

echo begin:%time%
adt -package -target ipa-app-store -useLegacyAOT no -storetype pkcs12 -keystore %KEY\_PATH% -storepass %KEY\_PASS% -provisioning-profile %KEY\_MOBILE\_PROVISION% %TARGET\_NAME% %FILE\_XML% %FILE\_SWF% %FILE\_LOGO% %FILE\_IPHONE4\_DEFAULT% %FILE\_IPHONE5\_DEFAULT% %FILE\_RES%
echo end:%time%
pause

\[/codesyntax\] 如果你用到了ane，别忘了加 -extdir . ！ 为什么注释写的也是英文呢？坑爹啊，批处理bat文件是ASNI编码格式，Fb里用的是UTF-8，本来像写中文的，结果要么编辑是中文乱码，要么运行时中文乱码，干脆拼几个英文单词算了！     后续：2014.03.06 这个功能目前还是测试版，打包demo或者小项目还是没问题的，但是打包公司的项目，各种bug，坐等adobe出正式版！