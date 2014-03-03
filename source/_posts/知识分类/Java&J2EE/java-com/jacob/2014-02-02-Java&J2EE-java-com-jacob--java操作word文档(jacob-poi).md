---
layout: post
title: "java操作word文档(jacob-poi)"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - java-com
 - jacob
--- 

# java操作word文档(jacob,poi)

项目需要，用户从系统里面下载word文档，该文档进行了填写限制和加密，用户只能在固定位置填写内容。现要求系统验证上传的附件是否从系统上下载下来的。

思路：系统上面的文档都加入一个固定书签，用户上传文档的时候，检验文档里是否包含这个书签。

采用jacob操作word文档

 

JACOB（java -com bridge）是一个 JAVA到微软的COM接口的桥梁。使用JACOB允许任何JVM访问COM对象，从而使JAVA应用程序能够调用COM对象。

下载地址：[http://sourceforge.net/projects/jacob-project/](http://sourceforge.net/projects/jacob-project/)

 

其中jacob-1.16.1-x64.dll 是用于64位机器上的，jacob-1.16.1-x86.dll用于32位的。

该dll放于 C:\Windows\system32 目录下。jacob.jar放于应用lib底下

测试代码
Java代码  [![收藏代码]()![]()]( "收藏这段代码")

1. ActiveXComponent    word = null;  
1. try {  
1.         word = new ActiveXComponent("Word.Application");  
1.         System.out.println("jacob当前版本："+word.getBuildVersion());  
1. }catch(Exception e ){  
1.          e.printStackTrace();  
1. }  

ActiveXComponent    word = null;

try {
        word = new ActiveXComponent("Word.Application");

        System.out.println("jacob当前版本："+word.getBuildVersion());
}catch(Exception e ){

         e.printStackTrace();
}

下面再贴出网上常见的代码+自己整理的几个方法（模糊查询书签等）

注意插入书签+书签值的方法，要先插入书签值再选中书签值，之后插入书签。这样根据书签名才能取得书签值。否则根据网络上很多方法，都取不到书签值或者取到空。因为书签值可以是一个点也可以是一大段内容。
Java代码  [![收藏代码]()![]()]( "收藏这段代码")

1. import java.io.File;  
1. import java.util.HashMap;  
1. import java.util.Map;  
1.   
1. import com.gdcn.bpaf.common.helper.StringHelper;  
1. import com.jacob.activeX.ActiveXComponent;  
1. import com.jacob.com.ComThread;  
1. import com.jacob.com.Dispatch;  
1. import com.jacob.com.Variant;  
1. /** 
1.  *  * 
1.  * <p>Description: {jacob操作word类}    </p> 
1.  * 
1.  * <p>Copyright: Copyright (c) 2011</p> 
1.  *  
1.  * <p>CreateDate: 2012-6-28</p> 
1.  * 
1.  * @author Beny 
1.  * @version 1.0 
1.  */  
1.   
1. public class JacobHelper {  
1.     // word文档  
1.     private Dispatch doc;  
1.   
1.     // word运行程序对象  
1.     private ActiveXComponent word;  
1.   
1.     // 所有word文档集合  
1.     private Dispatch documents;  
1.   
1.     // 选定的范围或插入点  
1.     private Dispatch selection;  
1.   
1.     private boolean saveOnExit = true;  
1.   
1.     public JacobHelper(boolean visible) throws Exception {  
1.         ComThread.InitSTA();//线程启动  
1.         if (word == null) {  
1.             word = new ActiveXComponent("Word.Application");  
1.             word.setProperty("Visible", new Variant(visible)); // 不可见打开word  
1.             word.setProperty("AutomationSecurity", new Variant(3)); // 禁用宏  
1.         }  
1.         if (documents == null)  
1.             documents = word.getProperty("Documents").toDispatch();  
1.     }  
1.   
1.     /** 
1.      * 设置退出时参数 
1.      *  
1.      * @param saveOnExit 
1.      *            boolean true-退出时保存文件，false-退出时不保存文件 
1.      */  
1.     public void setSaveOnExit(boolean saveOnExit) {  
1.         this.saveOnExit = saveOnExit;  
1.     }  
1.   
1.     /** 
1.      * 创建一个新的word文档 
1.      *  
1.      */  
1.     public void createNewDocument() {  
1.         doc = Dispatch.call(documents, "Add").toDispatch();  
1.         selection = Dispatch.get(word, "Selection").toDispatch();  
1.     }  
1.   
1.     /** 
1.      * 打开一个已存在的文档 
1.      *  
1.      * @param docPath 
1.      */  
1.     public void openDocument(String docPath) {  
1. //      closeDocument();  
1.         doc = Dispatch.call(documents, "Open", docPath).toDispatch();  
1.         selection = Dispatch.get(word, "Selection").toDispatch();  
1.     }  
1.   
1.     /** 
1.      * 只读方式打开一个加密的文档 
1.      *  
1.      * @param docPath-文件全名 
1.      * @param pwd-密码 
1.      */  
1.     public void openDocumentOnlyRead(String docPath, String pwd)  
1.             throws Exception {  
1. //      closeDocument();  
1.         doc = Dispatch.callN(  
1.                 documents,  
1.                 "Open",  
1.                 new Object[] { docPath, new Variant(false), new Variant(true),  
1.                         new Variant(true), pwd, "", new Variant(false) })  
1.                 .toDispatch();  
1.         selection = Dispatch.get(word, "Selection").toDispatch();  
1.     }  
1.     /** 
1.      * 打开一个加密的文档 
1.      * @param docPath 
1.      * @param pwd 
1.      * @throws Exception 
1.      */  
1.     public void openDocument(String docPath, String pwd) throws Exception {  
1. //      closeDocument();  
1.         doc = Dispatch.callN(  
1.                 documents,  
1.                 "Open",  
1.                 new Object[] { docPath, new Variant(false), new Variant(false),  
1.                         new Variant(true), pwd }).toDispatch();  
1.         selection = Dispatch.get(word, "Selection").toDispatch();  
1.     }  
1.   
1.     /** 
1.      * 从选定内容或插入点开始查找文本 
1.      *  
1.      * @param toFindText 
1.      *            要查找的文本 
1.      * @return boolean true-查找到并选中该文本，false-未查找到文本 
1.      */  
1.     @SuppressWarnings("static-access")  
1.     public boolean find(String toFindText) {  
1.         if (toFindText == null || toFindText.equals(""))  
1.             return false;  
1.         // 从selection所在位置开始查询  
1.         Dispatch find = word.call(selection, "Find").toDispatch();  
1.         // 设置要查找的内容  
1.         Dispatch.put(find, "Text", toFindText);  
1.         // 向前查找  
1.         Dispatch.put(find, "Forward", "True");  
1.         // 设置格式  
1.         Dispatch.put(find, "Format", "True");  
1.         // 大小写匹配  
1.         Dispatch.put(find, "MatchCase", "True");  
1.         // 全字匹配  
1.         Dispatch.put(find, "MatchWholeWord", "false");  
1.         // 查找并选中  
1.         return Dispatch.call(find, "Execute").getBoolean();  
1.     }  
1.   
1.     /** 
1.      * 把选定选定内容设定为替换文本 
1.      *  
1.      * @param toFindText 
1.      *            查找字符串 
1.      * @param newText 
1.      *            要替换的内容 
1.      * @return 
1.      */  
1.     public boolean replaceText(String toFindText, String newText) {  
1.         if (!find(toFindText))  
1.             return false;  
1.         Dispatch.put(selection, "Text", newText);  
1.         return true;  
1.     }  
1.   
1.     /** 
1.      * 全局替换文本 
1.      *  
1.      * @param toFindText 
1.      *            查找字符串 
1.      * @param newText 
1.      *            要替换的内容 
1.      */  
1.     public void replaceAllText(String toFindText, String newText) {  
1.         while (find(toFindText)) {  
1.             Dispatch.put(selection, "Text", newText);  
1.             Dispatch.call(selection, "MoveRight");  
1.         }  
1.     }  
1.   
1.     /** 
1.      * 在当前插入点插入字符串 
1.      *  
1.      * @param newText 
1.      *            要插入的新字符串 
1.      */  
1.     public void insertText(String newText) {  
1.         Dispatch.put(selection, "Text", newText);  
1.     }  
1.   
1.   
1.   
1.     /** 
1.      * 设置当前选定内容的字体 
1.      *  
1.      * @param boldSize 
1.      * @param italicSize 
1.      * @param underLineSize 
1.      *            下划线 
1.      * @param colorSize 
1.      *            字体颜色 
1.      * @param size 
1.      *            字体大小 
1.      * @param name 
1.      *            字体名称 
1.      * @param hidden 
1.      *            是否隐藏 
1.      */  
1.     public void setFont(boolean bold, boolean italic, boolean underLine,  
1.             String colorSize, String size, String name,boolean hidden) {  
1.         Dispatch font = Dispatch.get(selection, "Font").toDispatch();  
1.         Dispatch.put(font, "Name", new Variant(name));  
1.         Dispatch.put(font, "Bold", new Variant(bold));  
1.         Dispatch.put(font, "Italic", new Variant(italic));  
1.         Dispatch.put(font, "Underline", new Variant(underLine));  
1.         Dispatch.put(font, "Color", colorSize);  
1.         Dispatch.put(font, "Size", size);  
1.         Dispatch.put(font, "Hidden", hidden);  
1.     }  
1.   
1.   
1.     /** 
1.      * 文件保存或另存为 
1.      *  
1.      * @param savePath 
1.      *            保存或另存为路径 
1.      */  
1.     public void save(String savePath) {  
1.         Dispatch.call(Dispatch.call(word, "WordBasic").getDispatch(),  
1.                 "FileSaveAs", savePath);  
1.     }  
1.   
1.     /** 
1.      * 文件保存为html格式 
1.      *  
1.      * @param savePath 
1.      * @param htmlPath 
1.      */  
1.     public void saveAsHtml(String htmlPath) {  
1.         Dispatch.invoke(doc, "SaveAs", Dispatch.Method, new Object[] {  
1.                 htmlPath, new Variant(8) }, new int[1]);  
1.     }  
1.   
1.     /** 
1.      * 关闭文档 
1.      *  
1.      * @param val 
1.      *            0不保存修改 -1 保存修改 -2 提示是否保存修改 
1.      */  
1.     public void closeDocument(int val) {  
1.         Dispatch.call(doc, "Close", new Variant(val));//注 是documents而不是doc  
1.         documents = null;  
1.         doc = null;  
1.     }  
1.   
1.     /** 
1.      * 关闭当前word文档 
1.      *  
1.      */  
1.     public void closeDocument() {  
1.         if (documents != null) {  
1.             Dispatch.call(documents, "Save");  
1.             Dispatch.call(documents, "Close", new Variant(saveOnExit));  
1.             documents = null;  
1.             doc = null;  
1.         }  
1.     }  
1.   
1.     public void closeDocumentWithoutSave() {  
1.         if (documents != null) {  
1.             Dispatch.call(documents, "Close", new Variant(false));  
1.             documents = null;  
1.             doc = null;  
1.         }  
1.     }  
1.   
1.     /** 
1.      * 保存并关闭全部应用 
1.      *  
1.      */  
1.     public void close() {  
1.         closeDocument(-1);  
1.         if (word != null) {  
1. //          Dispatch.call(word, "Quit");  
1.             word.invoke("Quit", new Variant[] {});  
1.             word = null;  
1.         }  
1.         selection = null;  
1.         documents = null;  
1.         ComThread.Release();//释放com线程。根据jacob的帮助文档，com的线程回收不由java的垃圾回收器处理  
1.   
1.     }  
1.     /** 
1.      * 打印当前word文档 
1.      *  
1.      */  
1.     public void printFile() {  
1.         if (doc != null) {  
1.             Dispatch.call(doc, "PrintOut");  
1.         }  
1.     }  
1.   
1.     /** 
1.      * 保护当前档,如果不存在, 使用expression.Protect(Type, NoReset, Password) 
1.      *  
1.      * @param pwd 
1.      * @param type 
1.      *            WdProtectionType 常量之一(int 类型，只读)： 
1.      *            1-wdAllowOnlyComments  仅批注 
1.      *            2-wdAllowOnlyFormFields 仅填写窗体 
1.      *            0-wdAllowOnlyRevisions 仅修订 
1.      *            -1-wdNoProtection 无保护,  
1.      *            3-wdAllowOnlyReading 只读 
1.      *  
1.      */  
1.     public void protectedWord(String pwd,String type) {  
1.         String protectionType = Dispatch.get(doc, "ProtectionType").toString();  
1.         if (protectionType.equals("-1")) {  
1.             Dispatch.call(doc, "Protect", Integer.parseInt(type), new Variant(true),pwd);  
1.         }  
1.     }  
1.   
1.     /** 
1.      * 解除文档保护,如果存在 
1.      *  
1.      * @param pwd 
1.      *            WdProtectionType 常量之一(int 类型，只读)： 
1.      *            1-wdAllowOnlyComments  仅批注 
1.      *            2-wdAllowOnlyFormFields 仅填写窗体 
1.      *            0-wdAllowOnlyRevisions 仅修订 
1.      *            -1-wdNoProtection 无保护,  
1.      *            3-wdAllowOnlyReading 只读 
1.      *  
1.      */  
1.     public void unProtectedWord(String pwd) {  
1.         String protectionType = Dispatch.get(doc, "ProtectionType").toString();  
1.         if (!protectionType.equals("0")&&!protectionType.equals("-1")) {  
1.             Dispatch.call(doc, "Unprotect", pwd);  
1.         }  
1.     }  
1.     /** 
1.      * 返回文档的保护类型 
1.      * @return 
1.      */  
1.     public String getProtectedType(){  
1.         return Dispatch.get(doc, "ProtectionType").toString();  
1.     }  
1.   
1.     /** 
1.      * 设置word文档安全级别 
1.      *  
1.      * @param value 
1.      *            1-msoAutomationSecurityByUI 使用“安全”对话框指定的安全设置。 
1.      *            2-msoAutomationSecurityForceDisable 
1.      *            在程序打开的所有文件中禁用所有宏，而不显示任何安全提醒。 3-msoAutomationSecurityLow 
1.      *            启用所有宏，这是启动应用程序时的默认值。 
1.      */  
1.     public void setAutomationSecurity(int value) {  
1.         word.setProperty("AutomationSecurity", new Variant(value));  
1.     }  
1.   
1.    
1.    
1.    
1.     /** 
1.      * 在word中插入标签 labelName是标签名，labelValue是标签值 
1.      * @param labelName 
1.      * @param labelValue 
1.      */  
1.     public  void insertLabelValue(String labelName,String labelValue) {  
1.   
1.        Dispatch bookMarks = Dispatch.call(doc, "Bookmarks").toDispatch();  
1.        boolean isExist = Dispatch.call(bookMarks, "Exists", labelName).getBoolean();  
1.         if (isExist == true) {  
1.             Dispatch rangeItem1 = Dispatch.call(bookMarks, "Item", labelName).toDispatch();  
1.             Dispatch range1 = Dispatch.call(rangeItem1, "Range").toDispatch();  
1.             String bookMark1Value = Dispatch.get(range1, "Text").toString();  
1.             System.out.println("书签内容："+bookMark1Value);  
1.         } else {  
1.             System.out.println("当前书签不存在,重新建立!");  
1.             //TODO 先插入文字，再查找选中文字，再插入标签  
1.             this.insertText(labelValue);  
1. //          this.find(labelValue);//查找文字，并选中  
1.             this.setFont(true, true,true,"102,92,38", "20", "",true);  
1.             Dispatch.call(bookMarks, "Add", labelName, selection);  
1.             Dispatch.call(bookMarks, "Hidden", labelName);  
1.         }  
1.     }  
1.     /** 
1.      * 在word中插入标签 labelName是标签名 
1.      * @param labelName 
1.      */  
1.     public  void insertLabel(String labelName) {  
1.   
1.        Dispatch bookMarks = Dispatch.call(doc, "Bookmarks").toDispatch();  
1.        boolean isExist = Dispatch.call(bookMarks, "Exists", labelName).getBoolean();  
1.         if (isExist == true) {  
1.             System.out.println("书签已存在");  
1.         } else {  
1.             System.out.println("建立书签："+labelName);  
1.             Dispatch.call(bookMarks, "Add", labelName, selection);  
1.         }  
1.     }     
1.     /** 
1.      * 查找书签 
1.      * @param labelName 
1.      * @return 
1.      */  
1.     public boolean findLabel(String labelName) {  
1.        Dispatch bookMarks = Dispatch.call(doc, "Bookmarks").toDispatch();  
1.        boolean isExist = Dispatch.call(bookMarks, "Exists", labelName).getBoolean();  
1.        if (isExist == true) {  
1.             return true;  
1.         } else {  
1.             System.out.println("当前书签不存在!");  
1.             return false;  
1.         }  
1.     }  
1.     /** 
1.      * 模糊查找书签,并返回准确的书签名称 
1.      * @param labelName 
1.      * @return 
1.      */  
1.     public String findLabelLike(String labelName) {  
1.        Dispatch bookMarks = Dispatch.call(doc, "Bookmarks").toDispatch();  
1.        int count = Dispatch.get(bookMarks, "Count").getInt(); // 书签数  
1.        Dispatch rangeItem = null;  
1.        String lname = "";  
1.        for(int i=1;i<=count;i++){  
1.            rangeItem = Dispatch.call(bookMarks, "Item", new Variant(i)).toDispatch();  
1.            lname = Dispatch.call(rangeItem, "Name").toString();//书签名称  
1.            if(lname.startsWith(labelName)){//前面匹配  
1. //             return lname.replaceFirst(labelName, "");//返回后面值  
1.                return lname;  
1.            }  
1.        }  
1.        return "";  
1.     }  
1.     /** 
1.      * 模糊删除书签 
1.      * @param labelName 
1.      */  
1.     public void deleteLableLike(String labelName){  
1.         Dispatch bookMarks = Dispatch.call(doc, "Bookmarks").toDispatch();  
1.        int count = Dispatch.get(bookMarks, "Count").getInt(); // 书签数  
1.        Dispatch rangeItem = null;  
1.        String lname = "";  
1.        for(int i=1;i<=count;i++){  
1.            rangeItem = Dispatch.call(bookMarks, "Item", new Variant(i)).toDispatch();  
1.            lname = Dispatch.call(rangeItem, "Name").toString();//书签名称  
1.            if(lname.startsWith(labelName)){//前面匹配  
1.                Dispatch.call(rangeItem, "Delete");  
1.                count--;//书签已被删除，书签数目和当前书签都要相应减1，否则会报错:集合找不到  
1.                i--;  
1.            }  
1.        }  
1.     }  
1.     /** 
1.      * 获取书签内容 
1.      * @param labelName 
1.      * @return 
1.      */  
1.     public String getLableValue(String labelName){  
1.         if(this.findLabel(labelName)){  
1.             Dispatch bookMarks = Dispatch.call(doc, "Bookmarks").toDispatch();  
1.             Dispatch rangeItem1 = Dispatch.call(bookMarks, "Item", labelName).toDispatch();  
1.             Dispatch range1 = Dispatch.call(rangeItem1, "Range").toDispatch();  
1.             Dispatch font = Dispatch.get(range1, "Font").toDispatch();  
1.             Dispatch.put(font, "Hidden", new Variant(false)); //显示书签内容  
1.             String bookMark1Value = Dispatch.get(range1, "Text").toString();  
1.             System.out.println("书签内容："+bookMark1Value);  
1. //            font = Dispatch.get(range1, "Font").toDispatch();  
1. //              Dispatch.put(font, "Hidden", new Variant(true)); //隐藏书签内容  
1.             return bookMark1Value;  
1.         }  
1.         return "";  
1.     }  
1.   
1.    
1.     public static void main(String[] args) throws Exception {  
1.   
1.     }     
1.    
1. }  

import java.io.File;

import java.util.HashMap;
import java.util.Map;

 
import com.gdcn.bpaf.common.helper.StringHelper;

import com.jacob.activeX.ActiveXComponent;
import com.jacob.com.ComThread;

import com.jacob.com.Dispatch;
import com.jacob.com.Variant;

/**
 *  *

 * <p>Description: {jacob操作word类}    </p>
*

* <p>Copyright: Copyright (c) 2011</p>
*

* <p>CreateDate: 2012-6-28</p>
*

* @author Beny
* @version 1.0

*/
 

public class JacobHelper {
    // word文档

    private Dispatch doc;
 

    // word运行程序对象
    private ActiveXComponent word;

 
    // 所有word文档集合

    private Dispatch documents;
 

    // 选定的范围或插入点
    private Dispatch selection;

 
    private boolean saveOnExit = true;

 
    public JacobHelper(boolean visible) throws Exception {

        ComThread.InitSTA();//线程启动
        if (word == null) {

            word = new ActiveXComponent("Word.Application");
            word.setProperty("Visible", new Variant(visible)); // 不可见打开word

            word.setProperty("AutomationSecurity", new Variant(3)); // 禁用宏
        }

        if (documents == null)
            documents = word.getProperty("Documents").toDispatch();

    }
 

    /**
     * 设置退出时参数

     *
     * @param saveOnExit

     *            boolean true-退出时保存文件，false-退出时不保存文件
     */

    public void setSaveOnExit(boolean saveOnExit) {
        this.saveOnExit = saveOnExit;

    }
 

    /**
     * 创建一个新的word文档

     *
     */

    public void createNewDocument() {
        doc = Dispatch.call(documents, "Add").toDispatch();

        selection = Dispatch.get(word, "Selection").toDispatch();
    }

 
    /**

     * 打开一个已存在的文档
     *

     * @param docPath
     */

    public void openDocument(String docPath) {
//        closeDocument();

        doc = Dispatch.call(documents, "Open", docPath).toDispatch();
        selection = Dispatch.get(word, "Selection").toDispatch();

    }
 

    /**
     * 只读方式打开一个加密的文档

     *
     * @param docPath-文件全名

     * @param pwd-密码
     */

    public void openDocumentOnlyRead(String docPath, String pwd)
            throws Exception {

//        closeDocument();
        doc = Dispatch.callN(

                documents,
                "Open",

                new Object[] { docPath, new Variant(false), new Variant(true),
                        new Variant(true), pwd, "", new Variant(false) })

                .toDispatch();
        selection = Dispatch.get(word, "Selection").toDispatch();

    }
    /**

     * 打开一个加密的文档
     * @param docPath

     * @param pwd
     * @throws Exception

     */
    public void openDocument(String docPath, String pwd) throws Exception {

//        closeDocument();
        doc = Dispatch.callN(

                documents,
                "Open",

                new Object[] { docPath, new Variant(false), new Variant(false),
                        new Variant(true), pwd }).toDispatch();

        selection = Dispatch.get(word, "Selection").toDispatch();
    }

 
    /**

     * 从选定内容或插入点开始查找文本
     *

     * @param toFindText
     *            要查找的文本

     * @return boolean true-查找到并选中该文本，false-未查找到文本
     */

    @SuppressWarnings("static-access")
    public boolean find(String toFindText) {

        if (toFindText == null || toFindText.equals(""))
            return false;

        // 从selection所在位置开始查询
        Dispatch find = word.call(selection, "Find").toDispatch();

        // 设置要查找的内容
        Dispatch.put(find, "Text", toFindText);

        // 向前查找
        Dispatch.put(find, "Forward", "True");

        // 设置格式
        Dispatch.put(find, "Format", "True");

        // 大小写匹配
        Dispatch.put(find, "MatchCase", "True");

        // 全字匹配
        Dispatch.put(find, "MatchWholeWord", "false");

        // 查找并选中
        return Dispatch.call(find, "Execute").getBoolean();

    }
 

    /**
     * 把选定选定内容设定为替换文本

     *
     * @param toFindText

     *            查找字符串
     * @param newText

     *            要替换的内容
     * @return

     */
    public boolean replaceText(String toFindText, String newText) {

        if (!find(toFindText))
            return false;

        Dispatch.put(selection, "Text", newText);
        return true;

    }
 

    /**
     * 全局替换文本

     *
     * @param toFindText

     *            查找字符串
     * @param newText

     *            要替换的内容
     */

    public void replaceAllText(String toFindText, String newText) {
        while (find(toFindText)) {

            Dispatch.put(selection, "Text", newText);
            Dispatch.call(selection, "MoveRight");

        }
    }

 
    /**

     * 在当前插入点插入字符串
     *

     * @param newText
     *            要插入的新字符串

     */
    public void insertText(String newText) {

        Dispatch.put(selection, "Text", newText);
    }

 
 

 
    /**

     * 设置当前选定内容的字体
     *

     * @param boldSize
     * @param italicSize

     * @param underLineSize
     *            下划线

     * @param colorSize
     *            字体颜色

     * @param size
     *            字体大小

     * @param name
     *            字体名称

     * @param hidden
     *            是否隐藏

     */
    public void setFont(boolean bold, boolean italic, boolean underLine,

            String colorSize, String size, String name,boolean hidden) {
        Dispatch font = Dispatch.get(selection, "Font").toDispatch();

        Dispatch.put(font, "Name", new Variant(name));
        Dispatch.put(font, "Bold", new Variant(bold));

        Dispatch.put(font, "Italic", new Variant(italic));
        Dispatch.put(font, "Underline", new Variant(underLine));

        Dispatch.put(font, "Color", colorSize);
        Dispatch.put(font, "Size", size);

        Dispatch.put(font, "Hidden", hidden);
    }

 
 

    /**
     * 文件保存或另存为

     *
     * @param savePath

     *            保存或另存为路径
     */

    public void save(String savePath) {
        Dispatch.call(Dispatch.call(word, "WordBasic").getDispatch(),

                "FileSaveAs", savePath);
    }

 
    /**

     * 文件保存为html格式
     *

     * @param savePath
     * @param htmlPath

     */
    public void saveAsHtml(String htmlPath) {

        Dispatch.invoke(doc, "SaveAs", Dispatch.Method, new Object[] {
                htmlPath, new Variant(8) }, new int[1]);

    }
 

    /**
     * 关闭文档

     *
     * @param val

     *            0不保存修改 -1 保存修改 -2 提示是否保存修改
     */

    public void closeDocument(int val) {
        Dispatch.call(doc, "Close", new Variant(val));//注 是documents而不是doc

        documents = null;
        doc = null;

    }
 

    /**
     * 关闭当前word文档

     *
     */

    public void closeDocument() {
        if (documents != null) {

            Dispatch.call(documents, "Save");
            Dispatch.call(documents, "Close", new Variant(saveOnExit));

            documents = null;
            doc = null;

        }
    }

 
    public void closeDocumentWithoutSave() {

        if (documents != null) {
            Dispatch.call(documents, "Close", new Variant(false));

            documents = null;
            doc = null;

        }
    }

 
    /**

     * 保存并关闭全部应用
     *

     */
    public void close() {

        closeDocument(-1);
        if (word != null) {

//            Dispatch.call(word, "Quit");
            word.invoke("Quit", new Variant[] {});

            word = null;
        }

        selection = null;
        documents = null;

        ComThread.Release();//释放com线程。根据jacob的帮助文档，com的线程回收不由java的垃圾回收器处理
 

    }
    /**

     * 打印当前word文档
     *

     */
    public void printFile() {

        if (doc != null) {
            Dispatch.call(doc, "PrintOut");

        }
    }

 
    /**

     * 保护当前档,如果不存在, 使用expression.Protect(Type, NoReset, Password)
     *

     * @param pwd
     * @param type

     *            WdProtectionType 常量之一(int 类型，只读)：
     *            1-wdAllowOnlyComments  仅批注

     *            2-wdAllowOnlyFormFields 仅填写窗体
     *            0-wdAllowOnlyRevisions 仅修订

     *            -1-wdNoProtection 无保护,
     *            3-wdAllowOnlyReading 只读

     *
     */

    public void protectedWord(String pwd,String type) {
        String protectionType = Dispatch.get(doc, "ProtectionType").toString();

        if (protectionType.equals("-1")) {
            Dispatch.call(doc, "Protect", Integer.parseInt(type), new Variant(true),pwd);

        }
    }

 
    /**

     * 解除文档保护,如果存在
     *

     * @param pwd
     *            WdProtectionType 常量之一(int 类型，只读)：

     *            1-wdAllowOnlyComments  仅批注
     *            2-wdAllowOnlyFormFields 仅填写窗体

     *            0-wdAllowOnlyRevisions 仅修订
     *            -1-wdNoProtection 无保护,

     *            3-wdAllowOnlyReading 只读
     *

     */
    public void unProtectedWord(String pwd) {

        String protectionType = Dispatch.get(doc, "ProtectionType").toString();
        if (!protectionType.equals("0")&&!protectionType.equals("-1")) {

            Dispatch.call(doc, "Unprotect", pwd);
        }

    }
    /**

     * 返回文档的保护类型
     * @return

     */
    public String getProtectedType(){

        return Dispatch.get(doc, "ProtectionType").toString();
    }

 
    /**

     * 设置word文档安全级别
     *

     * @param value
     *            1-msoAutomationSecurityByUI 使用“安全”对话框指定的安全设置。

     *            2-msoAutomationSecurityForceDisable
     *            在程序打开的所有文件中禁用所有宏，而不显示任何安全提醒。 3-msoAutomationSecurityLow

     *            启用所有宏，这是启动应用程序时的默认值。
     */

    public void setAutomationSecurity(int value) {
        word.setProperty("AutomationSecurity", new Variant(value));

    }
 

 
 

 
    /**

     * 在word中插入标签 labelName是标签名，labelValue是标签值
     * @param labelName

     * @param labelValue
     */

    public  void insertLabelValue(String labelName,String labelValue) {
 

       Dispatch bookMarks = Dispatch.call(doc, "Bookmarks").toDispatch();
       boolean isExist = Dispatch.call(bookMarks, "Exists", labelName).getBoolean();

        if (isExist == true) {
            Dispatch rangeItem1 = Dispatch.call(bookMarks, "Item", labelName).toDispatch();

            Dispatch range1 = Dispatch.call(rangeItem1, "Range").toDispatch();
            String bookMark1Value = Dispatch.get(range1, "Text").toString();

              System.out.println("书签内容："+bookMark1Value);
        } else {

            System.out.println("当前书签不存在,重新建立!");
            //TODO 先插入文字，再查找选中文字，再插入标签

            this.insertText(labelValue);
//            this.find(labelValue);//查找文字，并选中

            this.setFont(true, true,true,"102,92,38", "20", "",true);
             Dispatch.call(bookMarks, "Add", labelName, selection);

             Dispatch.call(bookMarks, "Hidden", labelName);
        }

    }
    /**

     * 在word中插入标签 labelName是标签名
     * @param labelName

     */
    public  void insertLabel(String labelName) {

 
       Dispatch bookMarks = Dispatch.call(doc, "Bookmarks").toDispatch();

       boolean isExist = Dispatch.call(bookMarks, "Exists", labelName).getBoolean();
        if (isExist == true) {

              System.out.println("书签已存在");
        } else {

            System.out.println("建立书签："+labelName);
             Dispatch.call(bookMarks, "Add", labelName, selection);

        }
    }   

    /**
     * 查找书签

     * @param labelName
     * @return

     */
    public boolean findLabel(String labelName) {

       Dispatch bookMarks = Dispatch.call(doc, "Bookmarks").toDispatch();
       boolean isExist = Dispatch.call(bookMarks, "Exists", labelName).getBoolean();

       if (isExist == true) {
              return true;

        } else {
            System.out.println("当前书签不存在!");

            return false;
        }

    }
    /**

     * 模糊查找书签,并返回准确的书签名称
     * @param labelName

     * @return
     */

    public String findLabelLike(String labelName) {
       Dispatch bookMarks = Dispatch.call(doc, "Bookmarks").toDispatch();

       int count = Dispatch.get(bookMarks, "Count").getInt(); // 书签数
       Dispatch rangeItem = null;

       String lname = "";
       for(int i=1;i<=count;i++){

           rangeItem = Dispatch.call(bookMarks, "Item", new Variant(i)).toDispatch();
           lname = Dispatch.call(rangeItem, "Name").toString();//书签名称

           if(lname.startsWith(labelName)){//前面匹配
//               return lname.replaceFirst(labelName, "");//返回后面值

               return lname;
           }

       }
       return "";

    }
    /**

     * 模糊删除书签
     * @param labelName

     */
    public void deleteLableLike(String labelName){

        Dispatch bookMarks = Dispatch.call(doc, "Bookmarks").toDispatch();
       int count = Dispatch.get(bookMarks, "Count").getInt(); // 书签数

       Dispatch rangeItem = null;
       String lname = "";

       for(int i=1;i<=count;i++){
           rangeItem = Dispatch.call(bookMarks, "Item", new Variant(i)).toDispatch();

           lname = Dispatch.call(rangeItem, "Name").toString();//书签名称
           if(lname.startsWith(labelName)){//前面匹配

               Dispatch.call(rangeItem, "Delete");
               count--;//书签已被删除，书签数目和当前书签都要相应减1，否则会报错:集合找不到

               i--;
           }

       }
    }

    /**
     * 获取书签内容

     * @param labelName
     * @return

     */
    public String getLableValue(String labelName){

        if(this.findLabel(labelName)){
            Dispatch bookMarks = Dispatch.call(doc, "Bookmarks").toDispatch();

            Dispatch rangeItem1 = Dispatch.call(bookMarks, "Item", labelName).toDispatch();
            Dispatch range1 = Dispatch.call(rangeItem1, "Range").toDispatch();

            Dispatch font = Dispatch.get(range1, "Font").toDispatch();
            Dispatch.put(font, "Hidden", new Variant(false)); //显示书签内容

            String bookMark1Value = Dispatch.get(range1, "Text").toString();
              System.out.println("书签内容："+bookMark1Value);

//            font = Dispatch.get(range1, "Font").toDispatch();
//              Dispatch.put(font, "Hidden", new Variant(true)); //隐藏书签内容

              return bookMark1Value;
        }

        return "";
    }

 
 

    public static void main(String[] args) throws Exception {
 

    }   
 

}

 采用jacob方式操作文档，经常会出现卡机的现象，所以最后采用poi方式来操作书签。若单纯的操作书签用poi方式还是比较简单的，但要操作表格、文档格式之类的还是用jacob功能比较强大。
Java代码  [![收藏代码]()![]()]( "收藏这段代码")

1. InputStream input = null;  
1. File docFile = new File( fileName );  
1. HWPFDocument document = null;  
1. try{  
1.     input = new FileInputStream(docFile);//加载 doc 文档  
1.     document = new HWPFDocument(input);//文件流方式创建hwpf  
1.     Bookmarks bookmarks =  document.getBookmarks();//文档书签  
1.     for(int i=0,length=bookmarks.getBookmarksCount();i<length;i++){  
1.         bookmarkName = bookmarks.getBookmark(i).getName();  
1.         //.....  
1.     }  
1.       
1. }catch( Exception e){  
1.       
1. }finally{  
1.     if( null != input )  
1.             input.close();  
1. }  

InputStream input = null;

File docFile = new File( fileName );
HWPFDocument document = null;

try{
    input = new FileInputStream(docFile);//加载 doc 文档

    document = new HWPFDocument(input);//文件流方式创建hwpf
    Bookmarks bookmarks =  document.getBookmarks();//文档书签

    for(int i=0,length=bookmarks.getBookmarksCount();i<length;i++){
        bookmarkName = bookmarks.getBookmark(i).getName();

        //.....
    }

 
}catch( Exception e){

 
}finally{

    if( null != input )
            input.close();

}

 
