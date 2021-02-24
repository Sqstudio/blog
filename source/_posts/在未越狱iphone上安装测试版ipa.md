---
title: 在未越狱iPhone上安装测试版ipa
tags:
  - ios
  - ipa
id: '920'
categories:
  - - 移动开发
date: 2013-08-16 12:27:50
---

![](http://cc.cocimg.com/bbs/attachment/thumb/Fid_6/6_112520_10520fdb5c8a186.png)

对于未越狱的iPhone上安装测试版ipa，可通过itms-services协议来实现。 方法如下： 1.需要一个html文件,引导下载用户在线安装ipa \[codesyntax lang="php"\]

<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<html>
  <head>
    <title>一键安装掌上综调iPhone版</title>
  </head>

  <body>
        <a href='itms-services://?action=download-manifest&url=http://222.177.4.242/ios/d.plist（plist地址）'>安装app</a>
  </body>
</html>

\[/codesyntax\] 2.plist文件 \[codesyntax lang="php"\]

<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
   <key>items</key>
   <array>
       <dict>
           <key>assets</key>
           <array>
               <dict>
                   <key>kind</key>
                   <string>software-package</string>
                   <key>url</key>
                   <string>http://127.0.0.1/latest/ipa/tue.ipa（安装包的url）</string>
               </dict>
               <dict>

                   <key>kind</key>
                   <string>display-image</string>
                   <key>needs-shine</key>
                   <true/>
                   <key>url</key>
                   <string>图片的地址</string>
               </dict>
      <dict>
                   <key>kind</key>
                   <string>full-size-image</string>
                   <key>needs-shine</key>
                   <true/>
                   <key>url</key>
                   <string>图片的地址</string>
               </dict>
           </array>
           <key>metadata</key>
           <dict>
               <key>bundle-identifier</key>
               <string>com.xinchun（和ipa中的相同）</string>
               <key>bundle-version</key>
               <string>1.0.0</string>
               <key>kind</key>
               <string>software</string>
               <key>subtitle</key>
               <string>Tue</string>
               <key>title</key>
               <string>Tue</string>
           </dict>
       </dict>
   </array>
</dict>
</plist>

\[/codesyntax\] 3.使用iphone自带的safari浏览器，浏览http://222.177.4.242/ios/d.html文件，即可安装了。 注意：有的iPhone上访问到网页，点击链接没有反映，这时候要查看safari浏览器的设置了，看有没有禁用弹出窗口什么的 相关文章：  [iOS 7.1下itms-services在线安装失败的解决方法](http://blog.sqstudio.com/other/ios/955.html "详细阅读 iOS 7.1下itms-services在线安装失败的解决方法")