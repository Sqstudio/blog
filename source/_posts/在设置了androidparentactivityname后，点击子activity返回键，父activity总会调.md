---
title: '在设置了android:parentActivityName后，点击子Activity返回键，父Activity总会调用OnDestroy()的解决方案'
tags: []
id: '1089'
categories:
  - - Android开发
date: 2015-12-13 23:58:13
---

一个Activity在manifet里声明了android:parentActivityName；这时候通过Activity左上角的返回按钮点击返回， 启动声明的父Activity，总会先调用父Activity的OnDestroy方法，具体如下面所说：

```
    <activity
        android:name="com.example.helloworld.DisplayMessageActivity"
        android:label="@string/title_activity_display_message"
        android:parentActivityName="com.example.helloworld.MainActivity" >
        <meta-data
            android:name="android.support.PARENT_ACTIVITY"
            android:value="com.example.helloworld.MainActivity" />
    </activity>
```

DisplayMessageActivity为子Activity，而MainActivity为父Activity，点击

DisplayMessageActivity的左上角返回按钮的时候，调用逻辑如下：

```
MainActivity.onDestroy()
MainActivity.onCreate(null)
MainActivity.onStart()
```

解决方案是：

**为设置MainActivity属性android:launchMode=singleTop** 

顺便脑补android:parentActivityName的作用，就是为了左上角给子Activity加一个返回按钮，具体信息如下：

> **Android 4.1提高性能、增强用户体验**
> 
> 　　App 栈导航：通过设置android:parentActivityName改变回退栈的内容，如果栈中没有parentActivity，则合成栈，通过onPrepareNavigateUpTaskStack()改变parentActivity中的内容。