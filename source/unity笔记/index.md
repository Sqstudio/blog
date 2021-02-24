---
title: Unity笔记
id: '1324'
tags: []
categories:
  - - uncategorized
comments: false
date: 2017-07-05 18:09:39
---

# 1.二维夹角计算

UI上三个点，其中一个是原点(0,0),另外两个点A为起始点，B为当前点，计算当前点和起始点的夹角

> 方法1：通过Vector3.Cross判断正负，纯Vector2.Angle 只能给出夹角

```
float VectorAngle (Vector2 from, Vector2 to)
{
            float angle;
            Vector3 cross = Vector3.Cross (from, to);
            angle = Vector2.Angle (from, to);
            return cross.z > 0 ? -angle : angle;
}
```

> 方法2：三角函数，先计算两个点相对X轴夹角，相减，得到需要的夹角----老魏

```
float GetAngleByScreenPos (Vector2 screenPos)
{
Vector2 offsetPos = Vector2.zero - screenPos;
return Mathf.Atan2 (offsetPos.y, offsetPos.x) * Mathf.Rad2Deg;
}

```

# 2.方向上的点计算

UI上三个点，其中一个是原点O(0,0),另外两个点A为鼠标点击的屏幕点，求在OA方向上的B点 如下图，限制B点不超过圆圈，圆圈半径100. ![2017-04-26 11-44-09.png](https://attachments.tower.im/tower/16e0efe9f0eb464f97d5b1abfb4ea1a4?version=auto&filename=2017-04-26%2011-44-09.png) 首先转换坐标：

```
 Vector3 worldPos = UICamera.ScreenToWorldPoint(ScreenPos);//ScreenPos 即为 A点坐标
 Vector3 localPos = this.transform.parent.InverseTransformPoint (worldPos);
```

然后获取B点两种方法： A：射线法：

```
if (localPos.sqrMagnitude > MAX_DIS * MAX_DIS) {
Ray ray = new Ray (Vector3.zero, localPos);
localPos = ray.GetPoint (MAX_DIS);
}
```

B：向量法：

```
if (localPos.sqrMagnitude > MAX_DIS * MAX_DIS) 
localPos = localPos.normalized * MAX_DIS;

```

 

# 3.局部截屏

截屏局部需要以（Screen.width，Screen.height）来创建RenderTexture，然后以Texture2D的ReadPixels(rect,destX,destY)来读取局部信息，此时的坐标系是左下角为原点(0,0).如下图,如果要需要截取内框（绿色）区域，需要计算出A和B的屏幕坐标，B-A获取框的宽高，来得到new Rect(A.x,A.y,B.x-Ax,B.y-A.y),调用下面的CaptureCameras方法来获取相应区域的Texture2D。

> \*注:仅5.3.6版本，截图上下会有偏移，左右正常，截图时可先将图片调至纵向居中，截图后调回。

![2017-05-12 10-50-21.png](https://attachments.tower.im/tower/e32dcbdd44df46b399786637b4fa0e9a?version=auto&filename=2017-05-12%2010-50-21.png)

```
public static Texture2D CaptureCameras (Rect rect, params Camera[] cameras)
{
RenderTexture rt = new RenderTexture ((int)Screen.width, (int)Screen.height, 24);
//临时设置相关相机的targetTexture为rt, 并手动渲染相关相机
for (int i = 0; i < cameras.Length; i++) {
if (cameras [i] != null) {
cameras [i].targetTexture = rt;
cameras [i].Render ();
}
}

//激活这个rt, 并从中中读取像素。
RenderTexture.active = rt;
Texture2D screenShot = new Texture2D ((int)rect.width, (int)rect.height, TextureFormat.RGB24, false);
screenShot.ReadPixels (rect, 0, 0);   //注：这个时候，它是从RenderTexture.active中读取像素
screenShot.Apply ();

//重置相关参数，以使用camera继续在屏幕上显示
for (int i = 0; i < cameras.Length; i++) {
if (cameras [i] != null)
cameras [i].targetTexture = null;
}
RenderTexture.active = null;
rt.Release ();
GameObject.Destroy (rt);
rt = null;

return screenShot;
}

```

# 4.跟随手指拖拽来旋转镜头

使用场景，手指拖拽时镜头跟随手指移动,计算方法：

> 先计算整个屏幕宽度所对应的旋转角度，然后根据手指在屏幕上的横移距离来获得需要旋转的角度。

```
a.计算总角度（fieldOfView是摄像机的侧面垂直角度，横向角度会根据屏幕比例变化）
var m_TotalCameraScreenWAngle = Camera.main.fieldOfView * Screen.width / Screen.height;
b.计算需要移动的角度
var angle = pDragGesture.DeltaMove.x * m_TotalCameraScreenWAngle / Screen.width;
m_CameraTransform.localEulerAngles -= new Vector3(0, angle, 0);
```