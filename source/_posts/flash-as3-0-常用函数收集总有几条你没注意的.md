---
title: 'Flash AS3.0 常用函数收集,总有几条你没注意的'
tags:
  - 常用函数
id: '899'
categories:
  - - AS3探究
date: 2013-05-30 09:52:16
---

以下知识点，适合初级和中级Aser，大牛无视！

获得某个实例对象的类名：getQualifiedClassName (实例名字符串表达式);

stage.addEventListener(MouseEvent.MOUSE\_OVER, mouseOverHandle);

function mouseOverHandle(e:Event):void {

 trace("over");

 //   返回instance\*\*之类的 

 trace(e.target.name);

 //返回元件名 

 trace(getQualifiedClassName(e.target));

}

Math.random();

范围为0~1

如 果想要0~100的话就是

Math.random()\*100

随机出 \[n,m\]范围的随机数

function randRange(min:Number, max:Number):Number {

var randomNum:Number = Math.floor(Math.random() \* (max - min + 1)) + min;

return randomNum;

}

应用例子:

模拟投银币,即希望得到随机布尔值(true 或 false): randRange(0, 1);

模拟投骰子,即希望得到随机六个值: randRange(1, 6);

输出10个随机数

for (var i = 0; i < 10; i++) {

var n:Number = randRange(1, 10)

trace(n);

}

【把数字取至最近的小数点位,即指定精确度】

1\. 决定你要取的数字的小数点位数:例如,如果你想把90.337取成90.34,就表示你要取到两位小数点位,也就是说你想取至最近的0.01;

2\. 让输入值除以步骤1所选的数字(此例为0.01);

3\. 使用Math.round()把步骤2所计得的值取成最近的整数;

4\. 把步骤3所得的结果乘以步骤2用于除法的那个值.

例如,要把90.337取成两个小数点位数,可以使用:

trace(Math.round(90.337/0.01)\*0.01); //输出:90.34

【把数字取成一个整数的最接近倍数值】

例1,这样会把92.5取成5的最近倍数值:

trace(Math.round(92.5/5)\*5); //输出:95

例2,这样会把92.5取成10的最近倍数值:

trace(Math.round(92.5/10)\*10); //输出:90

【private,protected,internal,public访问权限】

private:只能在类本身内部访问,按惯例,命名私有成员时以下划线"\_"开头;

protected:可以由类本身或任何子类访问.但这是以实例为基础的.换言之,类实例可以访问自己的保护成员或者父类的保护成员,但不能访问相同类的其它实例的保护成员,按惯例,命名保护成员时以下划线"\_"开头;

internal:可以由类本身或者相同包内的任何类访问;

public:可以在类内部访问,也可以由类实例访问,或者声明为static时,可以直接从类访问.

【检查变量类型并返回布尔值】

is

【检查变量类型并返回类型】

typeof

【检查对象类型并返回该对象】

as

【Timer类注意事项】

不要认为Timer可以极其准确;使用Timer时间间隔不要低于10毫秒.

【一个函数具有未知个数的参数,用arguments对象或"...(rest)"符号访问它的参数】

注意:使用"...(rest)"参数会使 arguments 对象不可用;

private funciton average():void{

 trace(arguments.length); //输出参数的个数

 // arguments的类型是:object,但可以像访问数组一样去访问它

 trace(arguments\[1\]); //输出第二个参数

}

private function average(...argu):void{

 trace(argu\[1\]); //输出第二个参数,argu参数名是自定义的.

}

【隐式的取出方法(getter)和设定方法(setter)】

public function get count():uint {

 return \_count;

}

public function set count(value:uint):uint {

 if(value < 100){

 \_count = value;

 }else {

 throw Error();

 }

}

【确保类是绝不会有子类,使用final】

final public class Example{}

【super关键字的使用】

super(); //父类的构造函数,只能在类实例构造函数内部使用

super.propertyName; //调用父类的属性,属性需要声明为public或protected

super.methodName(); //调用父类的方法,方法需要声明为public或protected

【在数组中获取最小或最大值】

var scores:Array = \[10, 4, 15, 8\];

scores.sort(Array.NUMERIC);

trace("Minimum: " + scores\[0\]);

trace("Maximum: " + scores\[scores.length - 1\]);

【计算两点之间的距离】

勾股定理: c2 = a2 + b2

假设有两个影片剪辑mc1和mc2,则它们两点间的距离c为:

var c:Number = Math.sqrt(Math.pow(mc1.x - mc2.x, 2) + Math.pow(mc1.y - mc2.y, 2));

【指出容器的显示清单中有多少显示对象】

每个容器都有numChildren属性.

【关于TextField以垂直方式把文字摆在按钮表面中心点的小技巧】

textField.y = (\_height - textField.textHeight) / 2;

textField.y -= 2; //减2个像素以调整偏移量

【过滤文字输入】

TextField.restrict = "此处为可输入的内容";

field.restrict = "^此处为禁止输入的内容";

restrict属性支持一些类似正则表达式的样式:

field.restrict = "a-zA-z"; //只允许大小字母

field.restrict = "a-zA-z "; //只允许字母和空格

field.restrict = "0-9"; //只允许数字

field.restrict = "^abcdefg"; //除了小写字母abcdefg不允许外,其它都允许

field.restrict = "^a-z"; //所有小写字母都不允许,但是,其它内容都允许,包括大写字母

field.restrict = "0-9^5"; //只允许数字,但5例外

让restrict字符包含具有特殊意义的字母(例如-和^):

field.restrict = "0-9\\\\-"; //允许数字和破折号

field.restrict = "0-9\\\\^"; //允许数字和^

field.restrict = "0-9\\\\\\\\"; //允许数字和反斜杠

你也可以使用Unicode转义序列,指定允许的内容.例如:

field.restrict = "^\\u001A";

注意:ActionScript有区分大小写的,如果restrict属性设为abc,允许字母的大写形式(A,B和C)输入时会变成小写对待形式(a,b和c),反之亦然.restrict属性只影响用户可以输入的内容,脚本可将任何文本放入文本字段中.

【设定输入框的最大长度】

TextField.maxChars:int

【检测播放器版本】

flash.system.Capabilities.version

对于8.5版以前的任何Flash Player版本,这种方法都不适用.

【判断客户端系统】

flash.system.Capabilities.os

【检测播放器类型】

flash.system.Capabilities.playerType

可能的值有:

"StandAlone"，用于独立的 Flash Player

"External"，用于外部的 Flash Player 或处于测试模式下

"PlugIn"，用于 Flash Player 浏览器插件

"ActiveX"，用于 Microsoft Internet Explorer 使用的 Flash Player ActiveX 控件

【检测系统语言】

flash.system.Capabilities.language

【判断用户是否启用了IME(输入法编辑器)】

flash.system.IME.enabled

【检测屏幕的分辨率】

flash.system.Capabilities.screenResolutionX

flash.system.Capabilities.screenResolutionY

【把弹出窗口居中的算法】

X = (舞台宽/2)-(窗口宽/2)

Y = (舞台高/2)-(窗口高/2)

【控制影片配合Player的方式,包括缩放问题】

stage.scaleMode

可供选择值:flash.display.StageScaleMode

【舞台的对齐方式】

stage.align

可供选择值:flash.display.StageAlign

【隐藏Flash Player的右键菜单】

stage.showDefaultContextMenu = false;

【检测系统是否具有音频功能】

flash.system.Capabilities.hasAudio

【检测播放器是在具有MP3解码器的系统上运行,还是在没有MP3解码器的系统上运行】

flash.system.Capabilities.hasMP3

【检测播放器能 (true) 还是不能 (false) 播放流式视频】

flash.system.Capabilities.hasStreamingVideo

【检测播放器是在支持 (true) 嵌入视频的系统上运行，还是在不支持 (false) 嵌入视频的系统上运行】

flash.system.Capabilities.hasEmbeddedVideo

【检测播放器能 (true) 还是不能 (false) 对视频流（如来自 Web 摄像头的视频流）进行编码】

flash.system.Capabilities.hasVideoEncoder

【显示 Flash Player 中的"安全设置"面板】

flash.system.Security.showSettings();

可供选择项:flash.system.SecurityPanel