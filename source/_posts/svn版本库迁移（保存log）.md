---
title: SVN版本库迁移（保存Log）
tags:
  - svn
id: '1046'
categories:
  - - 周边技术
date: 2015-07-23 12:15:43
---

svn版本库完整迁移，保存完整信息：

> 关闭所有运行的进程，并确认没有程序在访问存储库（如 httpd、svnserve 或本地用户在直接访问）。 备份svn存储库 #压缩备份 svnadmin dump /home/workhome/svn/repository gzip > ~/repository-backup.gz #不压缩备份 svnadmin dump /home/workhome/svn/repository > ~/repository-backup.svn 恢复svn存储库 #建立新的svn存储库 svnadmin create /home/workhome/svn/newrepository #确认成功与否 ls -l /home/workhome/svn/newrepository #导入存储库数据 svnadmin load /home/workhome/svn/newrepository < ~/repository-backup.svn