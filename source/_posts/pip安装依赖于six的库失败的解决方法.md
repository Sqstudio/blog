---
title: Pip安装依赖于six的库失败的解决方法
tags:
  - pip
id: '1280'
categories:
  - - Python
date: 2017-02-22 19:13:50
---

今天在安装 google-api-python-client库时，six库一直报错升级不了

```
Installing collected packages: six
Found existing installation: six 1.4.1
DEPRECATION: Uninstalling a distutils installed project (six) has been deprecated and will be removed in a future version. This is due to the fact that uninstalling a distutils project will only partially uninstall the project.
Uninstalling six-1.4.1:
...
```

参考github上的解决办法：[https://github.com/pypa/pip/issues/3165](https://github.com/pypa/pip/issues/3165) 将安装命令改成如下即可：

```
sudo pip install --upgrade google-api-python-client --ignore-installed six
```

即使用参数--ignore-installed来忽略本地安装的six。

> 原因可能是Apple预安装的这个six库出于安全原因被设置为sudo也不可以执行操作，所以需要依赖于高版本的库就需要更新six，但是没有six的权限，所以就回报错。