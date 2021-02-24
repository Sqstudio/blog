---
title: 使LitJson支持Unity的类型
tags:
  - LitJson
  - Unity
id: '1332'
categories:
  - - Unity3D
date: 2017-07-07 16:51:32
---

在使用LitJson时，会发现不支持Unity的Vector3等类型，更为奇怪的是在Unity的Editor模式下是正常的，在真机设备上不行了，今天查看源码终于找到原因了。 LitJson本身为支持Unity已经定义了一个类UnityTypeBindings，局部代码如下：

#if UNITY\_EDITOR
\[UnityEditor.InitializeOnLoad\]
#endif
public static class UnityTypeBindings {

   static bool registerd;

   static UnityTypeBindings(){
      Register();
   }

就是因为在UNITY\_EDITOR下加了标签\[UnityEditor.InitializeOnLoad\]，所以在编辑器模式下支持Vector3等类型，因此只需要在项目加上如下代码即可：

UnityTypeBindings.Register()

参照以下注册方法，可实现自定义类型的Json序列化支持

Action<Vector2,JsonWriter> writeVector2 = (v,w) => {
   w.WriteObjectStart();
   w.WriteProperty("x",v.x);
   w.WriteProperty("y",v.y);
   w.WriteObjectEnd();
};

JsonMapper.RegisterExporter<Vector2>((v,w) => {
   writeVector2(v,w);
});

参考资料：
LitJson官网：[http://lbv.github.io/litjson/](http://lbv.github.io/litjson/)