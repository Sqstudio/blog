---
title: Mac环境下svn的使用
tags:
  - svn
id: '1048'
categories:
  - - 周边技术
date: 2015-06-23 12:18:15
---

在Windows环境中，我们一般使用TortoiseSVN来搭建svn环境。在Mac环境下，由于Mac自带了svn的服务器端和客户端功能，所以我们可以在不装任何第三方软件的前提下使用svn功能，不过还需做一下简单的配置。 我们首先来看下，如何在Mac环境下搭建svn服务器端环境。

## 创建代码仓库，用来存储客户端所上传的代码

我先在/User/apple目录下新建一个svn目录，以后可以在svn目录下创建多个仓库目录 打开终端，创建一个mycode仓库，输入指令：**svnadmin create /Users/apple/svn/mycode** 指令执行成功后，会发现硬盘上多了个/Users/apple/svn/mycode目录，目录结构如下： ![](http://images.cnitblog.com/blog/497279/201303/10002140-c4cf85b829bd477ea3a3779a4cd0d7fd.png)  

## 配置svn的用户权限

主要是修改/svn/mycode/conf目录下的三个文件 1.打开svnserve.conf，将下列配置项前面的#和空格都去掉

1.  # anon-access = read
2.  # auth-access = write

4.  # password-db = passwd

6.  # authz-db = authz

anon-access = read代表匿名访问的时候是只读的，若改为anon-access = none代表禁止匿名访问，需要帐号密码才能访问   2.打开passwd，在\[users\]下面添加帐号和密码，比如：

1.  \[users\]
2.  mj = 123
3.  jj = 456

帐号是mj，密码是123   3.打开authz，配置用户组和权限 我们可以将在passwd里添加的用户分配到不同的用户组里，以后的话，就可以对不同用户组设置不同的权限，没有必要对每个用户进行单独设置权限。 在\[groups\]下面添加组名和用户名，多个用户之间用逗号(,)隔开

1.  \[groups\]
2.  topgroup=mj,jj

说明mj和jj都是属于topgroup这个组的，接下来再进行权限配置。 使用\[/\]代表svn服务器中的所有资源库

1.  \[/\]
2.  @topgroup = rw

上面的配置说明topgroup这个组中的所有用户对所有资源库都有读写(rw)权限，组名前面要用@ 如果是用户名，不用加@，比如mj这个用户有读写权限

1.  \[/\]
2.  mj = rw

至于其他精细的权限控制，可以参考authz文件中的其他内容   4.启动svn服务器 前面配置了这么多，最关键还是看能否正常启动服务器，若启动不来，前面做再多工作也是徒劳。 在终端输入下列指令：**svnserve -d -r /Users/apple/svn** 或者输入：**svnserve -d -r /Users/apple/svn****/mycode** 没有任何提示就说明启动成功了   5.关闭svn服务器 如果你想要关闭svn服务器，最有效的办法是打开实用工具里面的“活动监视器”![](http://images.cnitblog.com/blog/497279/201303/10002641-8b49f872b6cd438c80f76f098d99db93.png) ![](http://images.cnitblog.com/blog/497279/201303/10002744-ce32c627b0b34f02a4067568bdc5e786.png) 综合上述，我们就可以轻松搭建svn服务器环境了  

## 使用svn客户端功能

1.从本地导入代码到服务器(第一次初始化导入) 在终端中输入 **svn import /Users/apple/Documents/eclipse\_workspace/weibo svn://localhost/mycode/weibo --username=mj --password=123 -m "初始化导入"** 我解释下指令的意思：将/Users/apple/Documents/eclipse\_workspace/weibo中的所有内容，上传到服务器mycode仓库的weibo目录下，后面双引号中的"初始化导入"是注释   2.从服务器端下载代码到客户端本地 在终端中输入**svn checkout svn://localhost/mycode --username=mj --password=123 /Users/apple/Documents/code** 我解释下指令的意思：将服务器中mycode仓库的内容下载到/Users/apple/Documents/code目录中   3.提交更改过的代码到服务器 在步骤2中已经将服务器端的代码都下载到/Users/apple/Documents/code目录中，现在修改下里面的一些代码，然后提交这些修改到服务器 1> 打开终端，先定位到/Users/apple/Documents/code目录，输入：**cd/Users/apple/Documents/code** 2> 输入提交指令：**svn commit -m "修改了main.m文件"** 这个指令会将/Users/apple/Documents/code下的所有修改都同步到服务器端，假如这次我只修改了main.文件 可以看到终端的打印信息：

1.  Sending        weibo/weibo/main.m
2.  Transmitting file data .
3.  Committed revision 2.

  4.更新服务器端的代码到客户端 这个应该是最简单的指令了，在终端中定位到客户端代码目录后，比如上面的/Users/apple/Documents/code目录，然后再输入指令：**svn update**   5.至于svn的其他用法，可以在终端输入：**svn help** **![](http://images.cnitblog.com/blog/497279/201303/10010442-37644dc2b0d54f7b8b421ede25d2f749.png)** 这里列出一大堆svn指令，后面括号中的内容的一般代表着指令的简称，比如我们可以用svn ci代替svn commit，用svn co代替svn checkout 原文链接：http://blog.csdn.net/q199109106q/article/details/8655204   后续追加： Flash Builder4.7安装了svn1.8插件后，或提示 Subversion native library not available，修改FB设置的svn里svn借口 [![QQ20150323-1@2x](http://qxu2059920095.my3w.com/blog/wp-content/uploads/2015/03/QQ20150323-1@2x.png)](http://qxu2059920095.my3w.com/blog/wp-content/uploads/2015/03/QQ20150323-1@2x.png) ---------- 添加svn服务开机启动 在etc/rc.local(要创建)文件里，加上svnserver -d -r user/apple/svn/rep(svn目录) 如果权限不足，需要 sudo ....