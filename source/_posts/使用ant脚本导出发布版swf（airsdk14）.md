---
title: 使用ANT脚本导出发布版SWF（AIRSDK14）
tags:
  - ant
id: '1004'
categories:
  - - 周边技术
date: 2014-05-26 00:07:52
---

使用ant脚本可以很方便的构建发布自己的项目，尤其是对于多语言多平台的版本，人工发布即费时，又容易漏掉东西，这些工作就交给脚本吧，机器就擅长干这个！ 网上的关于ant的资料更多是用flex的sdk，但是现在移动开发一般都是在FB4.7里用的airsdk，经博主尝试以后，基本做法是一样的，ant的安装就不赘述了，自行搜索即可。下面就贴出相关代码：   build.xml如下： \[codesyntax lang="actionscript3"\]

<?xml version="1.0" encoding="UTF-8"?>
<project name="My App Builder" basedir=".">

<property name="FLEX\_HOME" value="D:\\Program Files (x86)\\Adobe\\Adobe Flash Builder 4.7\\eclipse\\plugins\\com.adobe.flash.compiler\_4.7.0.349722\\AIRSDK" />
<taskdef resource="flexTasks.tasks" classpath="${FLEX\_HOME}/ant/lib/flexTasks.jar" />
<property name="ADT.JAR" value="${FLEX\_HOME}/lib/adt.jar" />

<property name="APP\_ROOT" value="D:\\jobWp47\\testant" />
<property name="APP\_NAME" value="testant" />
<property name="APP\_MAIN\_CLASS" value="testant.as" />
<property name="APP\_ROOT\_FILE" value="${APP\_NAME}.swf" />
<property name="APP\_OUTPUT\_PATH" value="${APP\_ROOT}/bin" />

<target name="swf">
<property name="release" value="true" />

<mxmlc file="${APP\_ROOT}/src/${APP\_MAIN\_CLASS}" output="${APP\_OUTPUT\_PATH}/${APP\_ROOT\_FILE}" actionscript-file-encoding="UTF-8" keep-generated-actionscript="true" show-unused-type-selector-warnings="false" static-link-runtime-shared-libraries="true" fork="true" warnings="false" incremental="true" maxmemory="512m">

<!-- Get default compiler options. -->
<load-config filename="${FLEX\_HOME}/frameworks/airmobile-config.xml" />
<!-- List of path elements that form the roots of ActionScript
            class hierarchies. -->
<source-path path-element="${FLEX\_HOME}/frameworks" />
<source-path path-element="${APP\_ROOT}/src" />

<!-- List of SWC files or directories that contaian SWC files. -->
<compiler.library-path dir="${FLEX\_HOME}/frameworks" append="true">
                <include name="libs" />
                <include name="../bundles/{locale}" />
            </compiler.library-path>
<swf-version>24</swf-version>
</mxmlc>

        <delete dir="${APP\_OUTPUT\_PATH}/generated"/>
        <delete>
            <fileset dir="${APP\_OUTPUT\_PATH}" includes="${APP\_NAME}.swf.cache"/>
        </delete>
</target>
</project>

\[/codesyntax\] 建的测试项目是 testant，相关路径设定需要根据实际情况调整 调用ant脚本，使用FB继承的ant，也很方便，不过博主发现，在控制台打印信息时，中文会乱码，暂时还没有找到解决方法，另外一种就是通过bat批处理来直接调用。 bat脚本如下：

> call ant -f build.xml swf

一行脚本即可搞定，build.xml 后跟的swf是xml文件里的target名，建议保存成bat文件，方便使用！
<!-- more -->
2014.6.10补充： 如果生成swf时候调用到ane，需要把ane改名为swc文件，以外部库的方式添加进来 从国外网站看到的，原文如下：

> I can't explain why, but if you copy and rename your .ane file and change them to .swc files (literally copy and rename, no conversion) and then reference the ANE's directory as an external library path -- it works. [原文链接](http://stackoverflow.com/questions/11112705/using-ant-mxmlc-task-with-native-extension)
> 
> <compiler.external-library-path dir="${ANE\_DIR}" includes="\*.swc" append="true" />