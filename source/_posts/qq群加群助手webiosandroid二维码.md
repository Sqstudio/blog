---
title: QQ群加群助手web/iOS/Android/二维码
tags: []
id: '1097'
categories:
  - - 周边技术
  - - 移动探索
date: 2016-01-09 10:31:54
---

官网地址：http://qun.qq.com/join.html 网页版：

> <a target="\_blank" href="http://shang.qq.com/wpa/qunwpa?idkey=【官网给的你的QQ群的Key】"><img border="0" src="http://pub.idqqimg.com/wpa/images/group.png" alt="Flash新手营内部学习" title="Flash新手营内部学习"></a>

iOS：

> \- (BOOL)joinGroup:(NSString \*)groupUin key:(NSString \*)key{ NSString \*urlStr = \[NSString stringWithFormat:@"mqqapi://card/show\_pslcard?src\_type=internal&version=1&uin=%@&key=%@&card\_type=group&source=external", @"244675613",@"【官网给的你的QQ群的Key】"\]; NSURL \*url = \[NSURL URLWithString:urlStr\]; if(\[\[UIApplication sharedApplication\] canOpenURL:url\]){ \[\[UIApplication sharedApplication\] openURL:url\]; return YES; } else return NO; }

  Android：

> /\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\* \* \* 发起添加群流程。群号：xxx key 为：【官网给的你的QQ群的Key】 \* 调用 joinQQGroup(【官网给的你的QQ群的Key】) 即可发起手Q客户端申请加群 \* \* @param key 由官网生成的key \* @return 返回true表示呼起手Q成功，返回fals表示呼起失败 \*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*/ public boolean joinQQGroup(String key) { Intent intent = new Intent(); intent.setData(Uri.parse("mqqopensdkapi://bizAgent/qm/qr?url=http%3A%2F%2Fqm.qq.com%2Fcgi-bin%2Fqm%2Fqr%3Ffrom%3Dapp%26p%3Dandroid%26k%3D" + key)); // 此Flag可根据具体产品需要自定义，如设置，则在加群界面按返回，返回手Q主界面，不设置，按返回会返回到呼起产品界面 //intent.addFlags(Intent.FLAG\_ACTIVITY\_NEW\_TASK) try { startActivity(intent); return true; } catch (Exception e) { // 未安装手Q或安装的版本不支持 return false; } }

    不过Android版貌似有个坑爹的地方，跳转到加群页面后，如果用户进行了操作，返回只会返回到QQ主界面，无法返回到自己应用，关键是再点击自己的应用图标  依然打开的是QQ，必须杀掉自己的应用才可以，坑！