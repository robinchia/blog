---
layout: post
title: "jacob合并几个word文件到一个word文件"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - java-com
 - jacob
--- 

# jacob合并几个word文件到一个word文件

     因项目需要将几个word文件合并到一个word文件，后面附项目运用的jar包jacob-1.9

jacob运用中，需要将附件内的jacob.dll放到windows/system32下

     直接上代码：
Java代码  [![收藏代码]()![]()]( "收藏这段代码")

1. public static void main(String[] args) {  
1.             List list  = new ArrayList();  
1.             String file1= "D:\\file1.doc";  
1.             String file2= "D:\\file2.doc";  
1.             String file3= "D:\\file3.doc";  
1.             list.add(file1);  
1.             list.add(file2);  
1.             list.add(file3);  
1.             uniteDoc(list,"d:\\file.doc");  
1.     }  
1.     public static void uniteDoc(List fileList, String savepaths) {  
1.         if (fileList.size() == 0 || fileList == null) {  
1.             return;  
1.         }  
1.         //打开word  
1.         ActiveXComponent app = new ActiveXComponent("Word.Application");//启动word  
1.         try {  
1.             // 设置word不可见  
1.             app.setProperty("Visible", new Variant(false));  
1.             //获得documents对象  
1.             Object docs = app.getProperty("Documents").toDispatch();  
1.             //打开第一个文件  
1.             Object doc = Dispatch  
1.                 .invoke(  
1.                         (Dispatch) docs,  
1.                         "Open",  
1.                         Dispatch.Method,  
1.                         new Object[] { (String) fileList.get(0),  
1.                                 new Variant(false), new Variant(true) },  
1.                         new int[3]).toDispatch();  
1.             //追加文件  
1.             for (int i = 1; i < fileList.size(); i++) {  
1.                 Dispatch.invoke(app.getProperty("Selection").toDispatch(),  
1.                     "insertFile", Dispatch.Method, new Object[] {  
1.                             (String) fileList.get(i), "",  
1.                             new Variant(false), new Variant(false),  
1.                             new Variant(false) }, new int[3]);  
1.             }  
1.             //保存新的word文件  
1.             Dispatch.invoke((Dispatch) doc, "SaveAs", Dispatch.Method,  
1.                 new Object[] { savepaths, new Variant(1) }, new int[3]);  
1.             Variant f = new Variant(false);  
1.             Dispatch.call((Dispatch) doc, "Close", f);  
1.         } catch (Exception e) {  
1.             throw new RuntimeException("合并word文件出错.原因:" + e);  
1.         } finally {  
1.             app.invoke("Quit", new Variant[] {});  
1.         }  
1.     }  

public static void main(String[] args) {

            List list  = new ArrayList();
            String file1= "D:\\file1.doc";

            String file2= "D:\\file2.doc";
            String file3= "D:\\file3.doc";

            list.add(file1);
            list.add(file2);

            list.add(file3);
            uniteDoc(list,"d:\\file.doc");

    }
    public static void uniteDoc(List fileList, String savepaths) {

        if (fileList.size() == 0 || fileList == null) {
            return;

        }
        //打开word

        ActiveXComponent app = new ActiveXComponent("Word.Application");//启动word
        try {

            // 设置word不可见
            app.setProperty("Visible", new Variant(false));

            //获得documents对象
            Object docs = app.getProperty("Documents").toDispatch();

            //打开第一个文件
            Object doc = Dispatch

                .invoke(
                        (Dispatch) docs,

                        "Open",
                        Dispatch.Method,

                        new Object[] { (String) fileList.get(0),
                                new Variant(false), new Variant(true) },

                        new int[3]).toDispatch();
            //追加文件

            for (int i = 1; i < fileList.size(); i++) {
                Dispatch.invoke(app.getProperty("Selection").toDispatch(),

                    "insertFile", Dispatch.Method, new Object[] {
                            (String) fileList.get(i), "",

                            new Variant(false), new Variant(false),
                            new Variant(false) }, new int[3]);

            }
            //保存新的word文件

            Dispatch.invoke((Dispatch) doc, "SaveAs", Dispatch.Method,
                new Object[] { savepaths, new Variant(1) }, new int[3]);

            Variant f = new Variant(false);
            Dispatch.call((Dispatch) doc, "Close", f);

        } catch (Exception e) {
            throw new RuntimeException("合并word文件出错.原因:" + e);

        } finally {
            app.invoke("Quit", new Variant[] {});

        }
    }

 
