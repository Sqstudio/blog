---
title: Linux安装配置MySql
tags:
  - linux
  - mysql
id: '1173'
categories:
  - - 服务器
date: 2016-12-21 18:24:58
---

# 1、安装客户端和服务器端

\[codesyntax lang="bash"\]

#确认mysql是否已安装
yum list installed mysql\*
rpm -qa  grep mysql\*

#查看是否有安装包
yum list mysql\*

#安装mysql客户端
yum install mysql

#安装mysql 服务器端
yum install mysql-server
yum install mysql-devel

\[/codesyntax\]  

# 2、启动、停止设置

数据库字符集设置 mysql配置文件/etc/my.cnf中加入default-character-set=utf8 \[codesyntax lang="bash"\]

#启动mysql服务：
service mysqld start
#或者
/etc/init.d/mysqld start

#设置开机启动：
chkconfig --add mysqld
chkconfig mysqld on

#查看开机启动设置是否成功
chkconfig --list  grep mysql\*
mysqld 0:关闭 1:关闭 2:启用 3:启用 4:启用 5:启用 6:关闭

#停止mysql服务：
service mysqld stop

\[/codesyntax\]    

# 3.创建或修改密码：

a.例如你的 root用户现在没有密码，你希望的密码修改为123456，那么命令是：

> mysqladmin -u root password 123456

b.如果你的root现在有密码了（123456），那么修改密码为abcdef的命令是：

> mysqladmin -u root -p password abcdef

注意，命令回车后会问你旧密码，输入旧密码123456之后命令完成，密码修改成功。

# 4.开启远程访问权限

  4.1、登陆mysql

> mysql -u root -p

4.2.修改mysql库的user表，将host项，从localhost改为%。%这里表示的是允许任意host访问，如果只允许某一个ip访问，则可改为相应的ip，比如可以将localhost改为192.168.1.123，这表示只允许局域网的192.168.1.123这个ip远程访问mysql。

> mysql> use mysql; mysql> update user set host = '%' where user = 'root'; mysql> select host, user from user; mysql> flush privileges;

# 5.防火墙开放3306端口

\[codesyntax lang="bash"\]

#打开防火墙配置文件
vi  /etc/sysconfig/iptables

#增加下面一行
-A INPUT -m state --state NEW -m tcp -p tcp --dport 3306 -j ACCEPT

#重启防火墙
service  iptables restart

\[/codesyntax\]  

# 6\. 查看mysql 状态

> \# ps -e grep mysqld
> 
> 13085 pts/1 00:00:00 mysqld\_safe
> 
> 13190 pts/1 00:00:00 mysqld

或者

> service mysqld status

# 7.修改mysql端口

为了安全，不采用默认端口3306，改为3506

7.1. 登录mysql

> \[root@test /\]# mysql -u root -p
> 
> Enter password:

7.2. 使用命令show global variables like 'port';查看端口号

> mysql> show global variables like 'port'; +---------------+-------+ Variable\_name Value +---------------+-------+ port 3306 +---------------+-------+

7.3. 修改端口，编辑/etc/my.cnf文件，早期版本有可能是my.conf文件名，增加端口参数，并且设定端口，注意该端口未被使用，保存退出。

> \[root@test etc\]# vi my.cnf \[mysqld\] port=3506 datadir=/var/lib/mysql socket=/var/lib/mysql/mysql.sock

7.4.重启mysql

> service mysqld restart

再次查看，端口已经变为3506

> mysql> show global variables like 'port'; +---------------+-------+ Variable\_name Value +---------------+-------+ port 3506 +---------------+-------+

# 8、mysql的几个重要目录

8.1数据库目录 /var/lib/mysql/ 8.2配置文件 /usr/share /mysql（mysql.server命令及配置文件） 8.3相关命令 /usr/bin（mysqladmin mysqldump等命令） 8.4启动脚本 /etc/rc.d/init.d/（启动脚本文件mysql的目录）