---
title: AIR p12转keystore证书签名apk
tags: []
id: '932'
categories:
  - - 移动探索
date: 2014-01-03 19:26:18
---

需求背景：向平台提交apk时提示该id已经存在，需要应用认领，可以认领的方式是使用java的keystore对平台提供的空白为签名apk进行签名上传验证，我们知道使用AIR开发的apk，所用的证书是p12的，如果进行签名认证呢？ 猜想一：试图将p12证书直接转成keystore，各种找资料，耗时半天，没成功！（会遇到找不到证书链的问题） 猜想二：反编译了平台提供的未签名apk，获取id，版本等，通过Flash Builder创建一个类似的apk，企图骗过平台，结果被识破。 继续查找，终于在某篇文章的评论处找到解决办法了。 正确方法：将p12证书直接导入到一个keystore文件中，就可以正常签名了！（真TM的） \[codesyntax lang="actionscript3"\]

//讲p12导入至keystore
keytool -v -importkeystore -srckeystore temp.p12 -srcstoretype PKCS12 -destkeystore temp.keystore -deststoretype JKS
//查看keystore
keytool -v -list -keystore temp.keystore
签名（1-证书链别名）
jarsigner -verbose -keystore d:\\key.keystore -signedjar d:\\signed.apk d:\\tap\_unsign.apk 1
删除keystore中别名为help.com的证书链
keytool -delete -alias help.com -keystore key.keystore

\[/codesyntax\] 签名后上传平台果然OK了！ 参考文章地址：[http://www.shadowkong.com/archives/1359](http://www.shadowkong.com/archives/1359) 这篇Rect的文章是介绍在adt命令里用keystore签名AIR生成的apk的方法，也很有用，建议还是通过keystore来进行签名，使AIR生成的apk更加接近java原生生成的！ 以下是Rect大神关于AIR生成的apk以及ANE等的研究：[https://github.com/recter/Anti-ADT/tree/master/RDT](https://github.com/recter/Anti-ADT/tree/master/RDT)（都很实用！）