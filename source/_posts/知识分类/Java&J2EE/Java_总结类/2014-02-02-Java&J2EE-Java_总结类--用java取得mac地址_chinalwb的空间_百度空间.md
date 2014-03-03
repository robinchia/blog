---
layout: post
title: "用java取得mac地址_chinalwb的空间_百度空间"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - Java_总结类
--- 

# 用java取得mac地址_chinalwb的空间_百度空间

### 分享到

* [QQ空间](http://hi.baidu.com/chinalwb/item/e6acf29f10fcb5c9b625317f#)
* [新浪微博](http://hi.baidu.com/chinalwb/item/e6acf29f10fcb5c9b625317f#)
* [百度搜藏](http://hi.baidu.com/chinalwb/item/e6acf29f10fcb5c9b625317f#)
* [人人网](http://hi.baidu.com/chinalwb/item/e6acf29f10fcb5c9b625317f#)
* [腾讯微博](http://hi.baidu.com/chinalwb/item/e6acf29f10fcb5c9b625317f#)
* [开心网](http://hi.baidu.com/chinalwb/item/e6acf29f10fcb5c9b625317f#)
* [腾讯朋友](http://hi.baidu.com/chinalwb/item/e6acf29f10fcb5c9b625317f#)
* [百度空间](http://hi.baidu.com/chinalwb/item/e6acf29f10fcb5c9b625317f#)
* [豆瓣网](http://hi.baidu.com/chinalwb/item/e6acf29f10fcb5c9b625317f#)
* [搜狐微博](http://hi.baidu.com/chinalwb/item/e6acf29f10fcb5c9b625317f#)
* [百度新首页](http://hi.baidu.com/chinalwb/item/e6acf29f10fcb5c9b625317f#)
* [QQ收藏](http://hi.baidu.com/chinalwb/item/e6acf29f10fcb5c9b625317f#)
* [和讯微博](http://hi.baidu.com/chinalwb/item/e6acf29f10fcb5c9b625317f#)
* [我的淘宝](http://hi.baidu.com/chinalwb/item/e6acf29f10fcb5c9b625317f#)
* [百度贴吧](http://hi.baidu.com/chinalwb/item/e6acf29f10fcb5c9b625317f#)
* [更多...](http://hi.baidu.com/chinalwb/item/e6acf29f10fcb5c9b625317f#)

[百度分享](http://hi.baidu.com/chinalwb/item/e6acf29f10fcb5c9b625317f#)

[](http://hi.baidu.com/)

* [首页](http://hi.baidu.com/home)
* [我的主页](http://hi.baidu.com/robinchia)
* [相册](http://hi.baidu.com/robinchia/albums)
* [广场](http://hi.baidu.com/index)
* [消息 ](http://hi.baidu.com/qmsg)
*
* [私信](http://msg.baidu.com/)
*
* [模板](http://hi.baidu.com/set/show/theme)
*
* [设置](http://hi.baidu.com/set/show/profile)
*
* [退出](https://passport.baidu.com/?logout&bdstoken=aa9855a9aad4216b27441b12182ad1a7&u=http://hi.baidu.com/chinalwb/item/e6acf29f10fcb5c9b625317f)
[关注此空间](http://hi.baidu.com/chinalwb/item/e6acf29f10fcb5c9b625317f#)
# [chinalwb的空间](http://hi.baidu.com/chinalwb)

扫描打印一体机 请找 QQ: 751510471， 329055754 EPSON ￥1,2500 长期合作 有优惠

2010-07-20 17:14

## 用java取得mac地址

项目中用到的一个方法
比较粗糙
没做整理
先记录下来
String getMacAddress() {
String macadd = "Any";
if (macaddress != null){
//System.out.println("Returning static macaddress");
return macaddress;
}
// On Solaris
if (Key.getRuntimeOS() == LicUtil.SUNOS){
String cmd = "/usr/bin/hostid";
BufferedReader br = null;
try {
Process p = Runtime.getRuntime().exec(cmd);
br = new BufferedReader(new InputStreamReader(p.getInputStream()));
String sLine = null;
if ((sLine = br.readLine()) != null) {
macadd = sLine.trim();
macaddress = macadd;
}
} catch (Exception ex) {
} finally {
try {
if (br != null)
br.close();
} catch (Exception ex) {}
}
} else if (Key.getRuntimeOS() == LicUtil.HPUX){
// On HP
String cmd = "/usr/sbin/lanscan -a";
BufferedReader br = null;
try {
Process p = Runtime.getRuntime().exec(cmd);
br = new BufferedReader(new InputStreamReader(p.getInputStream()));
String sLine = null;
if ((sLine = br.readLine()) != null) {
macadd = sLine.trim();
macaddress = macadd;
}
} catch (Exception ex) {
} finally {
try {
if (br != null)
br.close();
} catch (Exception ex) {}
}
} else if (Key.getRuntimeOS() == LicUtil.IBMAIX){
// On IBMAIX
String cmd = "/usr/bin/uname -m";
BufferedReader br = null;
try {
Process p = Runtime.getRuntime().exec(cmd);
br = new BufferedReader(new InputStreamReader(p.getInputStream()));
String sLine = null;
if ((sLine = br.readLine()) != null){
macadd = sLine.trim();
macaddress = macadd;
}
} catch (Exception ex) {
} finally {
try {
if (br != null)
br.close();
} catch (Exception ex) {}
}
} else if (Key.getRuntimeOS() == LicUtil.LINUX){
// On Linux
String cmd = "/sbin/ifconfig eth0";
BufferedReader br = null;
try {
Process p = Runtime.getRuntime().exec(cmd);
br = new BufferedReader(new InputStreamReader(p.getInputStream()));
String sLine = null;
if ((sLine = br.readLine()) != null) {
sLine = sLine.trim();
StringTokenizer st = new StringTokenizer(sLine);
while (st.hasMoreTokens()) {
String tmp = st.nextToken();
if (tmp.equals("HWaddr")) {
macadd = st.nextToken();
macaddress = macadd;
break;
}
}
}
// Add IP address - specail case on Linux
if ((sLine = br.readLine()) != null) {
sLine = sLine.trim();
StringTokenizer st = new StringTokenizer(sLine);
while (st.hasMoreTokens()) {
String tmp = st.nextToken();
if (tmp.startsWith("addr:")) {
setIP(tmp.substring(5));
break;
}
}
}
} catch (Exception ex) {
} finally {
try {
if (br != null)
br.close();
} catch (Exception ex) {}
}
} else if (Key.getRuntimeOS() == LicUtil.WIN){
// On Windows
// Yes, popup         String cmd = "cmd /c start /min ipconfig /all";
// 0519 more popup thane 0518   String cmd = "cmd /c ipconfig /all";
// 0518 Still see Popup from installer String cmd = "start /min ipconfig /all";
// Get Error in MacAddress         
// Pop Up Windows in installer
String cmd = "ipconfig /all";
BufferedReader br = null;
try {
Process p = Runtime.getRuntime().exec(cmd);
br = new BufferedReader(new InputStreamReader(p.getInputStream()));
String sLine = null;
while ((sLine = br.readLine()) != null) {
sLine = sLine.trim();
if (sLine.indexOf("Physical Address") != -1){
StringTokenizer winst = new StringTokenizer(sLine, ":");
winst.nextToken();
macadd = winst.nextToken().trim();
macaddress = macadd;
//System.out.println("Setting static macaddress");
break;
}
continue;
}
} catch (Exception ex) {
} finally {
try {
if (br != null)
br.close();
} catch (Exception ex) {}
}
}
return macadd;
}
[#Java](http://hi.baidu.com/tag/Java/feeds)

分享到： [](http://hi.baidu.com/chinalwb/item/e6acf29f10fcb5c9b625317f# "分享到QQ空间") [](http://hi.baidu.com/chinalwb/item/e6acf29f10fcb5c9b625317f# "分享到新浪微博") [](http://hi.baidu.com/chinalwb/item/e6acf29f10fcb5c9b625317f# "分享到腾讯微博") [](http://hi.baidu.com/chinalwb/item/e6acf29f10fcb5c9b625317f# "分享到人人网")

[举报](http://hi.baidu.com/chinalwb/item/e6acf29f10fcb5c9b625317f#) 浏览(213) [评论](http://hi.baidu.com/chinalwb/item/e6acf29f10fcb5c9b625317f#) [转载](http://hi.baidu.com/chinalwb/item/e6acf29f10fcb5c9b625317f#)

### 您可能也喜欢
[](http://hi.baidu.com/chinalwb/item/f3b6da5c40c12c12db16357f "上一篇")

[](http://hi.baidu.com/chinalwb/item/a726291024ee93403b176e7d "下一篇")

评论

 同时评论给 

 同时评论给原文作者 

[ 发布 ](http://hi.baidu.com/chinalwb/item/e6acf29f10fcb5c9b625317f#)

500/0

[收起](http://hi.baidu.com/chinalwb/item/e6acf29f10fcb5c9b625317f#)|[查看更多](http://hi.baidu.com/chinalwb/item/e6acf29f10fcb5c9b625317f#reply)

[帮助中心](http://help.baidu.com/question?prod_en=hi) | [空间客服](http://tieba.baidu.com/p/1781683739) | [投诉中心](http://tousu.baidu.com/hi/add) | [空间协议](http://www.baidu.com/search/hi_contract.html)

©2012 Baidu
[a](http://hi.baidu.com/pub/show/createmusic "发布音乐")

[b](http://hi.baidu.com/robinchia "我的主页")
[c](http://hi.baidu.com/home "首页")

[d](http://hi.baidu.com/pub/show/createtext "发布文字")
[e](http://hi.baidu.com/pub/show/createpic "发布图片")

[f](http://hi.baidu.com/pub/show/createvideo "发布视频")
