---
title: Mac 下使用tree命令
tags:
  - tree
  - 目录树
id: '1130'
categories:
  - - 周边技术
date: 2016-08-25 17:03:09
---

  [![tree](http://qxu2059920095.my3w.com/blog/wp-content/uploads/2016/08/tree.jpg)](http://qxu2059920095.my3w.com/blog/wp-content/uploads/2016/08/tree.jpg) 在Mac上，使用tree命令生成目录树，查看文件目录，非常直观方便。 首先，用brew来安装tree。

> brew install tree

安装过程如下：

\[codesyntax lang="php"\]

\==> Downloading https://homebrew.bintray.com/bottles/tree-1.7.0.el\_capitan.bottle.1.tar.gz

######################################################################## 100.0%

==> Pouring tree-1.7.0.el\_capitan.bottle.1.tar.gz

  /usr/local/Cellar/tree/1.7.0: 7 files, 113.1K

\[/codesyntax\] 安装完成后，使用tree命令查看下效果，-L 表示层级

> tree -L 2

上面命令即显示当前目录2层的目录树。 效果如下： [![tree2](http://qxu2059920095.my3w.com/blog/wp-content/uploads/2016/08/tree2.png)](http://qxu2059920095.my3w.com/blog/wp-content/uploads/2016/08/tree2.png)   还可以讲目录树保存一下。

> tree -L 2 > README.md