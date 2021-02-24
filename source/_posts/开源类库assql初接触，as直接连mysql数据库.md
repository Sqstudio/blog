---
title: 开源类库ASSQL初接触，AS直接连mysql数据库
tags: []
id: '608'
categories:
  - - AS3探究
date: 2012-02-25 19:42:53
---

刚发现这个强大的开源类库，assql，可以让as直接连mysql数据库，猛一下，我觉得豁然开朗，终于，与数据库打交道不需要通过后台了！ 之前看网上有人说中文乱码什么，这个问题我没有遇到，我用的是2.8版本，把数据库和表设置为utf-8，中文显示正常！ 先给出google项目地址：[http://code.google.com/p/assql/](http://code.google.com/p/assql/) google项目上有几个例子，不过都是Flex的，下面我给个简单纯as的小例子： \[codesyntax lang="actionscript3"\]

package
{
import com.maclema.mysql.Connection;
import com.maclema.mysql.MySqlToken;
import com.maclema.mysql.ResultSet;
import com.maclema.mysql.Statement;
import com.maclema.mysql.events.MySqlEvent;

import flash.display.Sprite;
import flash.events.Event;

public class MySqlTest extends Sprite
{

private var con:Connection;
public function MySqlTest()
{
con = new Connection("localhost", 3306, "root", "123456", "test");
con.addEventListener(Event.CONNECT, handleConnected);
con.connect();
}

private function handleConnected(e:Event):void {
var st:Statement = con.createStatement();
var token:MySqlToken = st.executeQuery("SELECT \* FROM user\_info");
token.addEventListener(MySqlEvent.RESULT,reslutHandler);
}

private function reslutHandler(e:MySqlEvent):void
{
var rs:ResultSet = e.resultSet;
rs.first();
for (var i:int = 0; i < rs.size(); i++) {
trace(rs.getString("userId"),rs.getString("userName"));
rs.next();
}
}

}
}

\[/codesyntax\]