---
title: Linux下安装ShadowSocks
tags:
  - linux
  - shadowsocks
id: '1164'
categories:
  - - 周边技术
  - - 服务器
date: 2016-12-21 14:34:43
---

服务器环境：CentOS6.5 ，Python3.5，服务器pip可用。 1.安装和配置：

> pip install shadowsocks

创建配置文件：

    vi /etc/shadowsocks.json

> {
>     "server":"0.0.0.0",
>     "server\_port":8989,
>     "local\_address": "127.0.0.1",
>     "local\_port":1080,
>     "password":"yourpassword",
>     "timeout":600,
>     "method":"aes-256-cfb",
>     "fast\_open": false,
>     "workers": 1
> }

各字段的含义：

*   server：服务器 IP (IPv4/IPv6)，注意这也将是服务端监听的 IP 地址
*   server\_port：监听的服务器端口
*   local\_address：本地监听的 IP 地址
*   local\_port：本地端端口
*   password：用来加密的密码
*   timeout：超时时间（秒）
*   method：加密方法，可选择 “bf-cfb”, “aes-256-cfb”, “des-cfb”, “rc4”, 等等。默认是一种不安全的加密，推荐用 “aes-256-cfb”
*   works：works数量，默认为 1
*   fast\_open：true 或 false。如果你的服务器 Linux 内核在3.7+，可以开启 fast\_open 以降低延迟。开启方法：

> echo 3 > /proc/sys/net/ipv4/tcp\_fastopen

开启之后，将 fast\_open 的配置设置为 true 即可。   **2、 命令行参数（服务器端启动命令）** ssserver -c /etc/shadowsocks.json 如果想在后台一直运行Shadowsocks，启动命令如下： nohup ssserver -c /etc/shadowsocks.json > /dev/null 2>&1 & **备注：**关于nohup，是可以让程序在后台运行的命令。 同时可以用命令行参数覆盖 /etc/shadowsocks.json 里的设置： sslocal -s 服务器地址 -p 服务器端口 -l 本地端端口 -k 密码 -m 加密方法 ssserver -p 服务器端口 -k 密码 -m 加密方法 **备注：**sslocal是客户端程序；ssserver是服务端程序。   **5、 防火墙设置（如有）** 编辑防火墙配置文件/etc/sysconfig/iptables，将服务器端口（server\_port）放行。 新增一条防火墙规则：

> \-A INPUT -m state --state NEW -m tcp -p tcp --dport 8989 -j ACCEPT

如果找不到 文件/etc/sysconfig/iptables， 可以使用命令行：

> iptables -A INPUT -m state --state NEW -m tcp -p tcp --dport 8989 -j ACCEPT service iptables save

重启防火墙iptables： service iptables restart 至此，服务器端的 Shadowsocks 安装和配置完毕。

* * *

等等...， 为什么能访问百度，访问不了facebook，啊？ 服务器是国内的！！！吐血三升！