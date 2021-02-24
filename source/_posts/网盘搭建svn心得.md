---
title: 网盘搭建svn心得
tags:
  - svn
id: '979'
categories:
  - - 周边技术
date: 2014-04-18 16:43:54
---

使用SVN保存自己的项目代码，还是很方便的，可惜现在的svn空间要么收费，要么免费但项目开源！ 最近看到一种用网盘搭建svn的方法，正好阿里云也到期了，不由动心一试！ 具体方法： 1.SVN的客户端相信都有，现在需要安装SVN的服务器端，（推荐[VisualSVN](http://www.visualsvn.com/server/download/)，小巧简单） 2.安装一个网盘客户端（360、百度、QQ微云等，什么网盘都可以），将[VisualSVN](http://www.visualsvn.com/server/download/)的仓库目录设置为网盘的同步目录！ OK，搞定了 ，就这么简单！ 原理：平时我们上传svn文件，只是通过网络上传到svn服务器，现在把本机作为一个svn的服务器，提交svn只是相当于把文件从一个目录复制到另一个目录，速度也快了很多；网盘的作用是把svn服务器的代码仓库上传到网盘服务器，这样即使在其他电脑上，只要把网盘里的文件同步下来，同样可以用svn，对于单个人来用，还是非常方便的！

[![](http://www.visualsvn.com/server/manager.png)](http://www.visualsvn.com/server/manager.png)