---
title: Linux(CentOS) 升级 Python 至 3.x
tags:
  - linux
  - python
id: '1159'
categories:
  - - Python
  - - 服务器
date: 2016-12-20 22:56:58
---

CentOS 6.5 中默认安装了 Python，版本比较低（2.6.6），为了使用新版 3.x，需要对旧版本进行升级。 由于很多基本的命令、软件包都依赖旧版本，比如：yum。所以，在更新 Python 时，建议不要删除旧版本（新旧版本可以共存）。  

> 1.查看 Python 版本号

当 Linux 上安装 Python 后（默认安装），只需要输入简单的命令，就可以查看 Python 的版本号： \[codesyntax lang="php"\]

python -V
Python 2.6.6

\[/codesyntax\]

> 2.下载新版本

进入 [Python下载页面](https://www.python.org/ftp/python/ "Python下载页面")，选择需要的版本。 这里，我选择的版本是 3.5.2 。 \[codesyntax lang="bash"\]

wget https://www.python.org/ftp/python/3.5.2/Python-3.5.2.tgz

\[/codesyntax\] 国外链接网速较慢，可以找下国内的资源。

> 3.解压安装

\[codesyntax lang="bash"\]

#解压
tar -zxvf Python-3.5.2.tgz

#进入目录
cd Python-3.5.2

#配置
./configure

make

make install

\[/codesyntax\]

> 4.验证

安装成功以后，就可以查看 Python 的版本了 \[codesyntax lang="bash"\]

\# python -V
Python 2.6.6
# python3 -V
Python 3.5.2

\[/codesyntax\] 一个是旧版本 2.x，另外一个是新版本 3.x。 **注意：**在 /usr/local/bin/ 下有一个 python3 的链接，指向 bin 目录下的 python 3.5。

> 5.设置 3.x 为默认版本

查看 Python 的路径，在 /usr/bin 下面。可以看到 python 链接的是 python 2.6，所以，执行 python 就相当于执行 python 2.6。 \[codesyntax lang="bash"\]

\# ls -al /usr/bin  grep python
-rwxr-xr-x.   1 root root       6832 Nov 23  2013 abrt-action-analyze-python
-rwxr-xr-x.   2 root root       4864 Jan 22  2014 python
lrwxrwxrwx.   1 root root          6 Aug 14  2014 python2 -> python
-rwxr-xr-x.   2 root root       4864 Jan 22  2014 python2.6

\[/codesyntax\] 修改链接： \[codesyntax lang="bash"\]

#将原来 python 的软链接重命名：
mv /usr/bin/python /usr/bin/python.bak

#将 python 链接至 python3：
ln -s /usr/local/bin/python3 /usr/bin/python

#再查看 Python 的版本
python -V
Python 3.5.2

\[/codesyntax\] 输出的是 3.x，说明已经使用的是 python3了  

> 6.配置yum

升级 Python 之后，由于将默认的 python 指向了 python3，yum 不能正常使用，需要编辑 yum 的配置文件： \[codesyntax lang="bash"\]

vim /usr/bin/yum

\[/codesyntax\] 将 #!/usr/bin/python 改为 #!/usr/bin/python2.7，保存退出即可   至此，终于大功告成了！！！