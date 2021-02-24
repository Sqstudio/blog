---
title: nginx、php-fpm、mysql用户权限解析
tags: []
id: '1192'
categories:
  - - 服务器
date: 2017-01-24 18:21:57
---

http://www.cnblogs.com/zrp2013/p/4183546.html

## 三者之间的关系

nginx本身不能处理PHP，它只是个web服务器。当接收到客户端请求后，如果是php请求，则转发给php解释器处理，并把结果返回给客户端。如果是静态页面的话，nginx自身处理，然后把结果返回给客户端。 Nginx下php解释器使用最多的就是fastcgi。一般情况nginx把php请求转发给fastcgi管理进程处理，fastcgi管理进程选择cgi子进程进行处理，然后把处理结果返回给nginx。 在这个过程中就牵涉到两个用户，一个是nginx运行的用户，一个是php-fpm运行的用户。如果访问的是一个静态文件的话，则只需要nginx运行的用户对文件具有读权限或者读写权限。 而如果访问的是一个php文件的话，则首先需要nginx运行的用户对文件有读取权限，读取到文件后发现是一个php文件，则转发给php-fpm，此时则需要php-fpm用户对文件具有有读权限或者读写权限。