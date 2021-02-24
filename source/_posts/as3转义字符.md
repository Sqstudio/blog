---
title: AS3转义字符
tags:
  - 转义字符
id: '456'
categories:
  - - AS3探究
date: 2011-08-08 13:18:45
---

[![](http://www.sqstudio.com/wp-content/uploads/2011/08/%E8%BD%AC%E4%B9%89%E5%AD%97%E7%AC%A6.jpg "转义字符")](http://www.sqstudio.com/wp-content/uploads/2011/08/%E8%BD%AC%E4%B9%89%E5%AD%97%E7%AC%A6.jpg) 常用转移字符：（其中 换页符、制表符也不太常用）

> 转义序列        转义结果
> 
>  \\b            空格符
>  \\f            换页符
>  \\n            换行符
>  \\r            回车符
>  \\t            制表符(Tab)
>  \\"            双引号
>  \\'            单引号
>  \\\\            反斜线符号(\\)

附上部分例子： 一、空格符。\\b

　　var str:String=”12345\\b67890″

　　会输出：12345 67890

二、换页符。\\f

　　var str:String=”12345\\f67890″

　　输出：12345 67890

三、换行符。\\n

　　var str:String=”12345\\n67890″;
 　 输出12345
 　　　 67890

四、回车符。\\r

　　var str:String=”12345\\r67890″

　　输出：12345（此处和换行符效果类似）
         67890

五、制表符。\\t

　　var str:String=”12345\\t67890″

　　输出：12345 　　67890（间隔变大）

八、双引号。\\” 九、单引号。\\’ 十、单个反斜杠字符。\\\\