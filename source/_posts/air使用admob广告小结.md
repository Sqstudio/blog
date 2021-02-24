---
title: AIR使用Admob广告小结
tags:
  - admob
  - Air
  - 广告
id: '813'
categories:
  - - 移动探索
date: 2013-01-10 20:32:36
---

最近在研究移动应用，现在Andriod做点应用试试，花了一点时间做了个[火车票查询](http://www.mumayi.com/android-264372.html)的应用，上了几个平台看看效果，跑跑流程。 顾名思义，应用是实时查询火车票的官网余票信息，应用做好了，免费的，就也想着放点广告试试，这时候就发现AS3的苦逼。搞广告需要ANE，还好在网上找到了一个Admob的广告ANE，是个[谷歌开源项目](http://code.google.com/p/flash-for-mobile/)，具体我就不详细说了，做着写的有例子，就是替换下jar文件，添加ANE到项目直接调用就可以了，自己简单封装了一个工具类，顺便分享下（有待完善）。 \[codesyntax lang="actionscript3"\]

package{
import so.cuo.anes.admob.AdAlign;
import so.cuo.anes.admob.AdEvent;
import so.cuo.anes.admob.AdType;
import so.cuo.anes.admob.Admob;

public class ADAdmob
{
/\*\*
 \*是否在显示广告
 \*/
public var isShowAd:Boolean=false;
private var myAdmobId:String = "**a150eeaece2a62e**";
private  static var m\_instance:ADAdmob=null;
public function ADAdmob(p:Param)
{
}

public static function get instance():ADAdmob{
return m\_instance=new ADAdmob(new Param());
}

/\*\*
 \*显示广告
 \* @param x  X坐标
 \* @param y   Y坐标
 \* @param type 广告类型
 \* @param isAlgin 是否按照align来部分，如果true 以align为准  xy无效  否则false 则以 xy为准 align无效
 \* @param align 位置
 \*
 \*/
public function showAd(x:int=0,y:int=0,type:String=AdType.BANNER,isAlgin:Boolean=false,align:int = AdAlign.ALIGN\_BOTTOM):void
{
var admob:Admob=Admob.getInstance();
if(admob.isSupported){
admob.setUnitId(myAdmobId);
admob.dispatcher.addEventListener(AdEvent.onReceiveAd,this.adHandler);
admob.dispatcher.addEventListener(AdEvent.onFailedToReceiveAd,this.ad2Handler);
admob.dispatcher.addEventListener(AdEvent.onDismissScreen,c1Handler);
admob.dispatcher.addEventListener(AdEvent.onPresentScreen,pressHandler);
admob.dispatcher.addEventListener(AdEvent.onLeaveApplication,levelHandler);
if(isAlgin){
admob.showRelation(align,type);
}else{
admob.show(x,y,type);
}
}else{
trace("not support");
}
}

/\*\*
 \*释放广告
 \*
 \*/
public function disposeAd():void{
var admob:Admob=Admob.getInstance();
admob.dispatcher.removeEventListener(AdEvent.onReceiveAd,this.adHandler);
admob.dispatcher.removeEventListener(AdEvent.onFailedToReceiveAd,this.ad2Handler);
admob.dispatcher.removeEventListener(AdEvent.onDismissScreen,c1Handler);
admob.dispatcher.removeEventListener(AdEvent.onPresentScreen,pressHandler);
admob.dispatcher.removeEventListener(AdEvent.onLeaveApplication,levelHandler);
admob.dispose();
isShowAd = false;
}

private function levelHandler(e:AdEvent):void
{
trace("level");
}

private function pressHandler(e:AdEvent):void
{
trace("press");

}

private function ad2Handler(e:AdEvent):void
{
trace("fail");
}

private function c1Handler(e:AdEvent):void
{
trace("Dismiss");
}

protected function adHandler(event:AdEvent):void
{
trace("receive ad");
isShowAd = true;
}
}
}
class Param{}

\[/codesyntax\] 广告类，还有待完善，有需要的朋友可以直接拿走，至于广告id，可以到admob上注册下，建个项目就有id了，很简单的。 下面说下，我用Admob的时候遇到的问题，我按照作者的方法，做了demo，广告却一直出不来，不知道怎么回事，折腾了好几天也没搞明白，在我近乎放弃admob的时候，终于发现原因了，原来是因为我的手机刷了rom，然后host文件了加了一万多条广告屏蔽记录，当然，admob的广告就显示不出来了，改了host，终于，看广告条了，鸡冻啊！！！ 如果你的广告也显示不出来，不妨看下是不是host被屏蔽了，另外360之类的手机杀毒软件也有可能会屏蔽你的广告。 下面是广告的效果，我直接截图我的应用： [![0](http://qxu2059920095.my3w.com/blog/wp-content/uploads/2013/01/0.png)](http://qxu2059920095.my3w.com/blog/wp-content/uploads/2013/01/0.png) [![train2](http://qxu2059920095.my3w.com/blog/wp-content/uploads/2013/01/train2.png)](http://qxu2059920095.my3w.com/blog/wp-content/uploads/2013/01/train2.png) 下面再贴一些相关的资料，有需要的朋友： admobANE：（Andriod+ios）[http://code.google.com/p/flash-for-mobile/](http://code.google.com/p/flash-for-mobile/) 作者建的群：56892018   感谢作者\[珠峰看雪\]的无私奉献！ 我的应用：火车票查询 [http://www.mumayi.com/android-264372.html](http://www.mumayi.com/android-264372.html)（木蚂蚁），实时查询火车票的，有兴趣的朋友也可以看下。 ANE作者的一篇admob的使用文章： flash 加载admob广告ane  [http://bbs.9ria.com/forum.php?mod=viewthread&tid=141414&fromuid=107509](http://bbs.9ria.com/forum.php?mod=viewthread&tid=141414&fromuid=107509) 还有一些其他的广告ANE：[http://ane.qiow.net/](http://www.qiow.net/) （这些没用过，有兴趣的朋友可以试试） 在使用广告的过程中有什么问题，也可以在此留言交流，抑或加作者的群一起讨论。