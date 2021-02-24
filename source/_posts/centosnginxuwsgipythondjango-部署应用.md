---
title: centos+nginx+uwsgi+python+django 部署应用
tags:
  - centos
  - django
  - nginx
  - python
  - uwsgi
id: '1111'
categories:
  - - Python
  - - 服务器
date: 2016-03-05 23:32:21
---

## 写在前面的话

折腾了近两天，终于把python小应用部署到阿里云服务器上了。作为一个运维小白，特记录一下部署过程，以备后续查验。 服务器选的阿里云，这里也有个和本文无关但是心酸的过程。因为以前用过阿里云，后来手机换号了，为了更换手机号简直心塞，整个流程简直死循环。大概是这样的：  

> 1.登陆阿里云账号，购买服务器
> 2.因为购买时候选择的是创建后设置服务器密码，所以购买完第一件事是重置服务器root密码
> 3.重置密码需要手机验证，但是老手机已经不用了
> 4.提交工单，申诉说明，客服让提交身份证件，手持身份证照等
> 5.身份证过期了，审核不通过
> 6.新身份证还没办下来，估计下个月才能拿到，但是服务器只买了一个月，到时候审核通过服务器也过期了

简直是心塞啊，幸好和客服沟通，可以用驾照代替，就这折腾两天，总算是把手机号给换了，泪奔！ 好了，言归正传，开始说部署！

# 一、系统选的CentOS

看阿里云官方推荐的CentOS，就直接选它了，本来选的是CentOS 6.5 ，结果安装后发现，自带的Python版本是2.6.6，貌似有点旧了，后来又重置了一次系统，换成了CentOS 7.0，自带的Python版本是2.7.5。 在刚发现CentOS6.5自带的Python为2.6.6时，也看了几篇文章，尝试把Python从升级到2.7，但是因为不知道哪里配错的原因，2.7虽然装好了，但是pip一直装不上，算了，只是为了部署个小python项目测试用，就不搞那么麻烦了，直接换成CentOS 7.0吧。 另外除了CentOS，Ubuntu应该也是不错的选择，以后有机会尝试一下。

# 二、安装nginx和python等相关包

### 2.1.安装nginx和python

方法很简单，yum搞定（感觉nginx已经自带了python，貌似不用安装python-devel **？？？）**

> sudo yum install epel-release sudo yum install python-devel nginx

先关闭 CentOS的防火墙，防止访问不了，后续再通过开放端口的方式进行细节处理

> 临时关闭防火墙 sudo systemctl stop firewalld

> 将 SELinux 设置为宽容模式，方便调试： sudo setenforce 0

**CentOS 7 iptables如何使用**：[http://stackoverflow.com/questions/24756240/](http://stackoverflow.com/questions/24756240/how-can-i-use-iptables-on-centos-7)

### 2.2.安装pip

> 1.通过wget下载py文件 wget https://bootstrap.pypa.io/get-pip.py 2.用python安装 python get-pip.py

wget 下载https文件，如果报错，可以加上--no-check-certificate

### 2.3.安装django，上传项目文件

> 目前django的版本是1.9.3 pip install django

pip install Django==1.5 即可安装其他（1.5）版本

> 上传文件（项目暂时放在root） scp local\_folder remote\_username@remote\_ip:remote\_folder

 

# 三、 使用  uwsgi 来部署（也可以用gunicorn）

### 3.1安装uwsgi

> pip install uwsgi

### 3.2启动uwsgi

执行uwsgi命令，启动

> uwsgi --http :8001 --chdir /path/to/project --home=/path/to/env --module project.wsgi

\--home 指定virtualenv 路径，如果没有可以去掉。project.wsgi 指 project/wsgi.py 文件 理论上这样就可以跑了，这时候访问你的服务器ip如（123.23.23.233:8001）,应该可以看到页面的 这时候查看端口占用（netstat -lpnt），会看到0.0.0.0:8001,已经有项目在监听。

### 3.3简化启动命令

但是这样的命令太长了，我们换一种，写ini文件来配置（或者xml/json/yaml） 在项目目录（/path/to/project）下，新建uwsgi.ini，内容如下： ss\[codesyntax lang="python"\]

\[uwsgi\]
socket = 127.0.0.1:8001
chdir = /root/python\_project/djtest/
wsgi-file = djtest/wsgi.py
touch-reload=/root/python\_project/djtest/reload

processes = 2
threads = 4

chmod-socket = 664
chown-socket = root:www-data

\[/codesyntax\]   我们执行以下命令，即可把项目部署到8001端口

> uwsgi --ini /root/python\_project/djtest/uwsgi.ini

#如果uwsgi无效，试试/path/to/uwsgi 即uwsgi的目录 注意，这里写的是“socket = 127.0.0.1:8001”，这里后面会和nginx关联到。 指定了touch-reload，执行如下命令，即可重新编译，重启应用

> touch /root/python\_project/djtest/reload

### 3.4端口占用，杀掉进程

如果端口被占用了，可以杀掉进程：

> probably another instance of uWSGI is running on the same address (:8001). bind(): Address already in use \[core/socket.c line 764\]

根据端口查找

> lsof -i :8001

可以查出

> COMMAND PID USER FD TYPE DEVICE             SIZE/OFF NODE NAME
> uwsgi  2208
> root 4u IPv4 0x53492abadb5c9659 0t0    TCP \*:teradataordbms (LISTEN)
> uwsgi  2209 root 4u IPv4 0x53492abadb5c9659 0t0    TCP \*:teradataordbms (LISTEN)

这时根据 PID 可以用下面的命令 kill 掉相关程序：

> sudo kill -9 2208 2209

 

# 4\. 使用supervisor来管理进程

安装 supervisor 软件包

> sudo pip install supervisor

生成 supervisor 默认配置文件，比如放在 /etc/supervisord.conf 路径中：

> sudo echo\_supervisord\_conf > /etc/supervisord.conf

打开sup，在底部添加如下配置

> \[program:djtest\]
> command=/usr/bin/uwsgi --ini /root/python\_project/djtest/uwsgi.ini
> directory=/root/python\_project/djtest
> user=root
> startsecs=0
> stopwaitsecs=0
> autostart=true
> autorestart=true

  command 中写上对应的命令，这样，就可以用 supervisor 来管理了。  

> 启动 supervisor
> sudo supervisord -c /etc/supervisord.conf
> 
> 重启 djtest 程序（项目）：
> sudo supervisorctl -c /etc/supervisord.conf restart
> djtest
> 
> 启动，停止，或重启 supervisor 管理的某个程序 或 所有程序：
> sudo supervisorctl -c /etc/supervisord.conf \[startstoprestart\] \[program-nameall\]

  通过重启程序，也可以实现项目的重新编译，重启生效。  

# 5.配置nginx

在/etc/nginx/nginx.conf配置里http下家如下server：

\[codesyntax lang="php"\]

    server {
        listen      80;
        server\_name 123.23.23.233;
        charset     utf-8;

        client\_max\_body\_size 75M;

        location / {
            uwsgi\_pass  127.0.0.1:8001;
            include     /etc/nginx/uwsgi\_params;
        }
    }

\[/codesyntax\]

重启nginx即可！ 重点：之前以uwsgi的方式启动，外网可以访问（123.23.23.233:8001）,但是nginx报错502，通过查看端口（netstat -lpnt）发现，以uwsgi --http :8001...的方式启动，监听的是0.0.0.0:8001，nginx无法转发过去，后直接把uwsgi启动至127.0.0.1:8001,然后nginx里的server配置也是uwsgi\_pass 127.0.0.1:8001;可以正常访问了，此时通过ip:8001是不能访问的。 另外，uwsgi\_pass可以用 unix:///tmp/zqxt.sock; 项目中uwsgi.ini的socket配置对应，也可以正常工作，但未实验成功。 作为一个运维小白，参考教程，一步一步出错排查，排查出错，总算跑起来了，虽然还有很多漏洞，但是足以欣慰。 参考资料：http://www.ziqiangxuetang.com/django/django-nginx-deploy.html     [![aliyun](http://blog.sqstudio.com/wp-content/uploads/2016/03/aliyun-300x185.jpg)](http://qxu2059920095.my3w.com/blog/wp-content/uploads/2016/03/aliyun-e1487147439130.jpg)