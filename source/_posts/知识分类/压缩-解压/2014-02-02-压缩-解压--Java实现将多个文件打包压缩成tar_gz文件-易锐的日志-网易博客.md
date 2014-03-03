---
layout: post
title: "Java实现将多个文件打包压缩成tar_gz文件 - 易锐的日志 - 网易博客"
categories: 压缩-解压
tags: 
 - 压缩-解压
--- 

# Java实现将多个文件打包压缩成tar_gz文件 - 易锐的日志 - 网易博客

[网易 ](http://blog.163.com/redirect.html?frompersonalbloghome&url=http://www.163.com/)

[新闻](http://blog.163.com/redirect.html?frompersonalbloghome&url=http://news.163.com/)[微博](http://blog.163.com/redirect.html?frompersonalbloghome&url=http://t.163.com/)[邮箱](http://blog.163.com/redirect.html?frompersonalbloghome&url=http://email.163.com/)[相册](http://blog.163.com/redirect.html?frompersonalbloghome&url=http://photo.163.com/)[阅读](http://blog.163.com/redirect.html?frompersonalbloghome&url=http://yuedu.163.com/)[有道](http://blog.163.com/redirect.html?frompersonalbloghome&url=http://www.youdao.com/)[摄影](http://blog.163.com/redirect.html?frompersonalbloghome&url=http://photo.163.com/pp/square/)[爱拍](http://blog.163.com/redirect.html?frompersonalbloghome&url=http://aipai.163.com?from=163blog)[闪电邮](http://blog.163.com/redirect.html?frompersonalbloghome&url=http://fm.163.com/?fmblogzx0628)[手机邮](http://m.123.163.com/?frombkcp)[印像派](http://blog.163.com/redirect.html?frompersonalbloghome&url=http://yxp.163.com/)[梦幻人生](http://blog.163.com/redirect.html?frompersonalbloghome&url=http://dream.163.com/?from=bkcp) [更多](http://blog.163.com/redirect.html?frompersonalbloghome&url=http://sitemap.163.com/)
 

[博客 ](http://blog.163.com/?frompersonalbloghome)

[手机博客](http://blog.163.com/services/wapblog.html?frompersonalbloghome)[博客搬家](http://blog.163.com/clone/clone.html?frompersonalbloghome)[博客VIP服务](http://blog.163.com/vip.do?frompersonalbloghome) [LiveWriter写博](http://blog.163.com/blog_admin/blog/static/721279201082863728829/?frompersonalbloghome)[word写博](http://blog.163.com/blog_admin/blog/static/721279201092713253500/?frompersonalbloghome)[邮件写博](http://blog.163.com/services/mailblog.html?frompersonalbloghome)[短信写博](http://blog.163.com/services/emsblog.html?frompersonalbloghome) [群博客](http://blog.163.com/showMultiUser.do?frompersonalbloghome)[博客油菜地](http://blog.163.com/BLOG_YOUCAI/)[博客话题](http://blog.163.com/ht/?frompersonalbloghome)[博客热点](http://blog.163.com/hot/history/?frompersonalbloghome)[博客圈子](http://q.163.com/#home?frompersonalbloghome)[找朋友](http://blog.163.com/findFriend.do?type=5?frompersonalbloghome)
[发现](http://blog.163.com/public/explore/?frompersonalbloghome)

[小组](http://blog.163.com/g/?frompersonalbloghome)
[风格](http://blog.163.com/public/theme/?frompersonalbloghome)

[](http://blog.163.com/randomUser.do "随便看看")
 

[博客VIP服务](http://blog.163.com/vip.do)

[创建博客](http://reg.163.com/reg/reg.jsp?product=blog&url=http://blog.163.com/ntesRegBlank.html&loginurl=http://blog.163.com/) [登录](http://blog.163.com/kliumin@126/blog/static/8165118520091063306587/#)  

 关注

重要提醒：系统检测到您的帐号可能存在被盗风险，请尽快[查看风险提示](http://reg.163.com/Main.jsp?username=kliumin@126.com)，并立即[修改密码](http://blog.163.com/kliumin@126/settings/?fromnewcenter#m=0&t=7)。  |  [关闭](http://blog.163.com/kliumin@126/blog/static/8165118520091063306587/)

网易博客安全提醒：系统检测到您当前密码的安全性较低，为了您的账号安全，建议您适时修改密码    [立即修改](http://blog.163.com/kliumin@126/settings/?fromnewcenter#m=0&t=7)  |  [关闭](http://blog.163.com/kliumin@126/blog/static/8165118520091063306587/)
[](http://blog.163.com/kliumin@126/blog/static/8165118520091063306587/)   [显示下一条](http://blog.163.com/kliumin@126/blog/static/8165118520091063306587/)  |  [关闭](http://blog.163.com/kliumin@126/blog/static/8165118520091063306587/)

[](http://blog.163.com/public/explore/?fromblogbanner)
[](http://www.lofter.com/invite/163blog/?email=)
# 易锐的博客

我的博客
 [](http://blog.163.com/kliumin@126/blog/static/8165118520091063306587/#)

## 导航

* [首页](http://blog.163.com/kliumin@126/)
* [日志](http://blog.163.com/kliumin@126/blog/)
* [相册](http://blog.163.com/kliumin@126/album/)
* [音乐](http://blog.163.com/kliumin@126/music/)
* [收藏](http://blog.163.com/kliumin@126/collection/)
* [博友](http://blog.163.com/kliumin@126/friends/)
* [关于我](http://blog.163.com/kliumin@126/profile/)
 

## 日志

 

[eclipse注释规范设置](http://blog.163.com/kliumin@126/blog/static/81651185200910413622503/)

### Java实现将多个文件打包压缩成tar.gz文件  

2009-11-06 15:30:06|  分类： [默认分类](http://blog.163.com/kliumin@126/blog/#m=0&t=1&c=fks_087067087087088066084083085095092087087070085087094070 "默认分类") |  标签： |字号大中小 [订阅]()

后缀为tar.gz的文件实际上时先将文件（单个或多个）打包成后缀为tar的（tar包）文件，再用gzip压缩成gz文件，如此来说我们便可以用两步来实现此功能，请看代码：

import java.io.File;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;
import java.util.zip.GZIPOutputStream;

import org.apache.commons.compress.archivers.tar.TarArchiveEntry;
import org.apache.commons.compress.archivers.tar.TarArchiveOutputStream;
import org.apache.commons.compress.utils.IOUtils;

/**
 * @Title: GZIPUtil.java
 * @Description: gzip文件压缩和解压缩工具类
 * @author LM
 * @date 2009-11-4 下午06:23:29
 * @version V1.0
 */
public class GZIPUtil {
 
 /**
  *
  * @Title: pack
  * @Description: 将一组文件打成tar包
  * @param sources 要打包的原文件数组
  * @param target 打包后的文件
  * @return File    返回打包后的文件
  * @throws
  */
 public static File pack(File[] sources, File target){
  FileOutputStream out = null;
  try {
   out = new FileOutputStream(target);
  } catch (FileNotFoundException e1) {
   e1.printStackTrace();
  }
  TarArchiveOutputStream os = new TarArchiveOutputStream(out);
  for (File file : sources) {
   try {
    os.putArchiveEntry(new TarArchiveEntry(file));
    IOUtils.copy(new FileInputStream(file), os);
    os.closeArchiveEntry();
    
   } catch (FileNotFoundException e) {
    e.printStackTrace();
   } catch (IOException e) {
    e.printStackTrace();
   }
  }
  if(os != null) {
   try {
    os.flush();
    os.close();
   } catch (IOException e) {
    e.printStackTrace();
   }
  }
  
  return target;
 }

 /**
  *
  * @Title: compress
  * @Description: 将文件用gzip压缩
  * @param  source 需要压缩的文件
  * @return File    返回压缩后的文件
  * @throws
  */
 public static File compress(File source) {
  File target = new File(source.getName() + ".gz");
  FileInputStream in = null;
  GZIPOutputStream out = null;
  try {
   in = new FileInputStream(source);
   out = new GZIPOutputStream(new FileOutputStream(target));
   byte[] array = new byte[1024];
   int number = -1;
   while((number = in.read(array, 0, array.length)) != -1) {
    out.write(array, 0, number);
   }
  } catch (FileNotFoundException e) {
   e.printStackTrace();
   return null;
  } catch (IOException e) {
   e.printStackTrace();
   return null;
  } finally {
   if(in != null) {
    try {
     in.close();
    } catch (IOException e) {
     e.printStackTrace();
     return null;
    }
   }
   
   if(out != null) {
    try {
     out.close();
    } catch (IOException e) {
     e.printStackTrace();
     return null;
    }
   }
  }
  return target;
 }

 public static void main(String[] args) {
  File[] sources = new File[] {new File("task.xml"), new File("app.properties")};
  File target = new File("release_package.tar");
  compress(pack(sources, target));
 }
}

可以打包压缩，那么解压缩就是反其道而行，有时间再来写一篇解压缩的。
  评论这张

![]() 转发至微博
![]() 转发至微博

![]() 0人  |  分享到：         

阅读(674)| 评论(0)| 引用 (0) |举报

 

 

[eclipse注释规范设置](http://blog.163.com/kliumin@126/blog/static/81651185200910413622503/)
### 历史上的今天

### 相关文章
### 最近读者

### 评论

[]()

this.p={ m:2, b:2, id:'fks_083066081080089069080083087095092087087070085087094070', blogTitle:'Java实现将多个文件打包压缩成tar.gz文件', blogAbstract:'<P\>后缀为tar.gz的文件实际上时先将文件（单个或多个）打包成后缀为tar的（tar包）文件，再用gzip压缩成gz文件，如此来说我们便可以用两步来实现此功能，请看代码：</P\>\r\n<P\>imp<wbr\>ort java.io.File;<BR\>imp<wbr\>ort java.io.FileInputStream;<BR\>imp<wbr\>ort java.io.FileNotFoundException;<BR\>imp<wbr\>ort java.io.FileOutputStream;<BR\>imp<wbr\>ort java.io.IOException;<BR\>imp<wbr\>ort java.util.zip.GZIPOutputStream;</P\>\r\n<P\>imp<wbr\>ort org.apache.commons.compress.archivers.tar.TarArchiveEntry;</P\>', blogTag:'', blogUrl:'blog/static/8165118520091063306587', isPublished:1, istop:false, type:0, modifyTime:0, publishTime:1257492606587, permalink:'blog/static/8165118520091063306587', commentCount:0, mainCommentCount:0, recommendCount:0, bsrk:-100, publisherId:0, recomBlogHome:false, attachmentsFileIds:[], vote:{}, groupInfo:{}, friendstatus:'none', followstatus:'unFollow', pubSucc:'', visitorProvince:'广东', visitorCity:'深圳', postAddInfo:{}, mset:'000', mcon:'', srk:-100, remindgoodnightblog:false, isBlackVisitor:false, isShowYodaoAd:false, hostIntro:'', hmcon:'' }   {list a as x} {if !!x} <div class="iblock nbw-fce nbw-f40"> <a class="fc03 noul" target="_blank" hidefocus="true" href="http://blog.163.com/${x.visitorName}/"> {if x.visitorName==visitor.userName} <img alt="${x.visitorNickname|escape}" onerror="location.f40" class="cwd bdwa bdc0" src="${fn1(x.visitorName)}&r=${visitor.imageUpdateTime}"/> {else} <img alt="${x.visitorNickname|escape}" onerror="location.f40" class="cwd bdwa bdc0" src="${fn1(x.visitorName)}"/> {/if} </a> <div class="cwd vname thide"> {if x.moveFrom=='wap'} <a class="noul pnt" target="_blank" href="http://blog.163.com/services/wapblog.html?frompersonalbloghome"><span title="来自网易手机博客" class="iblock wapIcon"> </span></a> {elseif x.moveFrom=='iphone'} <a class="noul pnt" target="_blank"><span title="来自iPhone客户端" class="iblock iphoneIcon"> </span></a> {elseif x.moveFrom=='android'} <a class="noul pnt" target="_blank"><span title="来自Android客户端" class="iblock androidIcon"> </span></a> {elseif x.moveFrom=='mobile'} <a class="noul pnt" target="_blank" href="http://blog.163.com/services/emsblog.html?frompersonalbloghome"><span title="来自网易短信写博" class="iblock wapIcon"> </span></a> {/if} <a class="fc03 m2a" target="_blank" hidefocus="true" href="http://blog.163.com/${x.visitorName}/"> ${fn(x.visitorNickname,8)|escape} </a> </div> </div> {/if} {/list}   {if !!a} <a target="_blank" href="http://blog.163.com/${a.userName}/"><img class="bdwa bdc0 pnt" onerror="location.f60" src="${fn1(a.userName)}"/></a> <a target="_blank" class="fc03 m2a" href="http://blog.163.com/${a.userName}/">${fn(a.nickname,8)|escape}</a> <div class="intro fc05">${a.selfIntro|escape}{if great260}${suplement}{/if}</div> <div class="acts ztag"></div> <div class="mbga phide xtag"> <div class="mbgai"> </div> <a class="fc03 xtag m2a" href="#" target="_blank"></a> </div> {/if}  <#--最新日志，群博日志-->  {list a as x} {if !!x} <li class="thide"><a target="_blank" class="fc03 m2a" href="${furl()}${x.permalink}/?latestBlog">${fn(x.title,26)|escape}</a></li> {/if} {/list}  <#--推荐日志-->  <p class="fc06">推荐过这篇日志的人：</p> <div> {list a as x} {if !!x} <div class="iblock nbw-fce nbw-f40"> <a class="fc03 noul" target="_blank" hidefocus="true" href="http://blog.163.com/${x.recommenderName}/"> <img alt="${x.recommenderNickname}" onerror="location.f40" class="cwd bdwa bdc0" src="${fn1(x.recommenderName)}"/> </a> <div class="cwd thide"> <a class="fc03 m2a" target="_blank" hidefocus="true" href="http://blog.163.com/${x.recommenderName}/"> ${fn(x.recommenderNickname,6)|escape} </a> </div> </div> {/if} {/list} </div> {if !!b&&b.length>0} <p class="fc06">他们还推荐了：</p> <ul> {list b as y} {if !!y} <li class="rrb"><span class="iblock">·</span><a class="fc03 m2a" target="_blank" href="http://blog.163.com/${y.recommendBlogPermalink}/?from=blog/static/8165118520091063306587">${y.recommendBlogTitle}</a></li> {/if} {/list} </ul> {/if}  <#--引用记录-->  <span class="pleft fc07">引用记录：</span> <ul class="tbac"> {list d as x} <li class="clearfix"> <span class="pleft iblock">·</span> <div class="tbl thide pleft"><span><a target="_blank" class="fc07 m2a" href="${x.referBlogUrl}">${x.referBlogTitle}</a></span></div> <div class="tbr pleft"><span><a target="_blank" class="fc07 m2a" href="${x.referHomePage}">${x.referUserName}</a></span></div> </li> {/list} </ul>  <#--博主推荐-->  {list a as x} {if !!x} <li class="thide"><a target="_blank" class="fc03 m2a" href="http://blog.163.com/${x.userName}/${x.permalink}/?recommendBlog" title="${x.title|default:""|escape}">${x.title|default:""|escape}</a></li> {/if} {/list}  <#--随机阅读-->  {list a as x} {if !!x} <li class="thide"><a target="_blank" class="fc03 m2a" href="http://blog.163.com/${x.userName}/${x.permalink}/?personalRecomBlog" title="${x.title|default:""|escape}">${x.title|default:""|escape}</a></li> {/if} {/list}  <#--首页推荐-->  {list a as x} {if !!x} <li class="thide"><a target="_blank" class="fc03 m2a" target="_blank" href="${x.blogUrl|default:""|escape}?recommendReader" title="${x.blogTile|default:""|escape}">${x.blogTile|default:""|escape}</a></li> {/if} {/list}  <#--相关文章-->  <ul> {list a as x} {if x_index>9}{break}{/if} {if !!x} <li class="thide fc03"> <a class="m2a" target="_blank" target="_blank" href="${x.url|default:""}?fromdm&fromSearch&isFromSearchEngine=${isFromSearchEngine|default:"no"}" title="${x.title|default:""|escape}">${x.title|default:""|escape}</a><span class="fc07">${fn2(parseInt(x.date),'yyyy-MM-dd HH:mm:ss')}</span> </li> {/if} {/list} </ul>  <#--历史上的今天-->  <ul> {list a as x} {if x_index>4}{break}{/if} {if !!x} <li class="thide fc03"> <a class="m2a" target="_blank" href="http://blog.163.com/${x.userName}/${x.permalink|default:""}" title="${x.title|default:""|escape}">${fn1(x.title,60)|escape}</a><span class="fc07">${fn2(x.publishTime,'yyyy-MM-dd HH:mm:ss')}</span> </li> {/if} {/list} </ul>  <#--右边模块结构-->  <div class="uinfo ztag"></div> <h4 class="fc07 fs0 ltt">最新日志</h4> <ul class="ztag blst"></ul> <h4 class="fc07 fs0 ltt phide ztag">该作者的其他文章</h4> <ul class="ztag blst"></ul> <h4 class="fc07 fs0 ltt phide ztag">博主推荐</h4> <ul class="ztag blst"></ul> <h4 class="fc07 fs0 ltt phide ztag">相关日志</h4> <ul class="ztag blst"></ul> <h4 class="fc07 fs0 ltt phide ztag">随机阅读</h4> <ul class="ztag blst"></ul> <h4 class="fc07 fs0 ltt phide ztag">首页推荐</h4> <ul class="ztag blst"></ul> <div class="more"><a target="_blank" class="fc03 m2a" href="http://blog.163.com">更多>></a></div> <br/><a id="yodaoad_suggest" target="_blank" href="http://fankui.163.com/ft/comment.fb?pid=12004&fid=16023" class="fc05 m2a phide">对“推广广告”提建议</a> <br/><br/> <div id="yodaoad_r" style="display:none;_zoom:1;"></div>  <#--评论模块结构-->  <div class="publish ztag"></div> <div class="bdwt bds2 bdc0 phide" id="yodaoad_2" style="_zoom:1;"></div> <div class="ztag bdwt bds2 bdc0"> <div class="case"></div> <div class="clearfix"></div> </div>  <#--引用模块结构-->  <div class="close"> <span class="ztag iblock icn0 icn0-57"> </span> </div> <div class="ztag phide"></div>  <#--博主发起的投票-->  {list a as x} {if !!x} <li> <a href="http://blog.163.com/${x.userName}/" target="_blank" class="m2a fc03">${x.nickName|escape}</a>  投票给 {var first_option = true;} {list x.voteDetailList as voteToOption} {if voteToOption==1} {if first_option==false},{/if}  “${b[voteToOption_index]}”   {/if} {/list} {if (x.role!="-1") },“我是${c[x.role]}”  {/if}     <span class="fc07">    ${fn1(x.voteTime)}</span> {if x.userName==''}{/if} {/if} {/list}
 

## 页脚

[公司简介](http://corp.163.com/index_gb.html) - [联系方法](http://gb.corp.163.com/gb/contactus.html) - [招聘信息](http://corp.163.com/gb/job/job.html) - [客户服务](http://help.163.com/?b13abk1) - [隐私政策](http://corp.163.com/gb/legal/legal.html) - [博客风格](http://blog.163.com/public/theme/) - [手机博客](http://blog.163.com/services/wapblog.html) - [VIP博客](http://blog.163.com/vip.do) - [订阅此博客]()

网易公司版权所有 ©1997-2011

<a rel="nofollow" class="pr" target="_blank" href="http://help.163.com/special/007525FT/blog.html?b13aze1">帮助</a> <span class="fr iblock space icn1 icn1-4"> </span> <a class="pr" target="_blank" href="http://blog.163.com/activation.do?host=activation&&username=${u}">${u}</a>   {list wl as x} <div class="grp">${x.g}</div> {list x.l as y} <a class="itm noul" href="#" hidefocus="true" name="{if typeof(y.v)=='string'}${y.v}{else}${y_index}{/if}">${y.n}</a> {/list} {/list}   {if defined('wl')} {list wl as x}<a class="itm noul" href="#" hidefocus="true" name="${x.v}">${x.n}</a>{/list} {/if}
