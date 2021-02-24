---
title: 猥琐的绕开AS3的crossdomain.xml权限
tags: []
id: '583'
categories:
  - - AS3探究
date: 2011-12-27 10:06:57
---

本文目前只针对这种情况：对方API可供JS等其他语言访问，但是由于缺少crossdomain.xml文件而造成As3无法访问 原理说明：AS3通过请求php传入link（网址）参数，由php请求link，并将数据返回给AS3 举例：如搜狐的ip接口http://pv.sohu.com/cityjson，由于确实crossdomain.xml文件而不能访问（当然，你在本地是可以访问的），这时候用过访问php，由php返回响应的数据给AS3 php文件如下： \[codesyntax lang="php" lines="no" capitalize="no"\]

<?php
 $link = $\_POST\['link'\];
 echo file\_get\_contents($link);
?>

\[/codesyntax\]   好吧，给个地址出来 http://wannianrili.duapp.com/getHttpContent.php   ，如果你发现这个地址，那就之后自己建个咯