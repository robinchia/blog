---
layout: post
title: "Apache FTP 实用代码"
categories: ftp
tags: 
 - ftp
--- 

# Apache FTP 实用代码

## 结合网上两个使用apache commons-net包的两个优秀的代码，结合实际，写出了如下的FtpUtil，供参考！
参考网址：[http://www.iteye.com/topic/173786](http://www.iteye.com/topic/173786)，[http://hi.baidu.com/victorlin23/blog/item/edc62a35dbae8a1a91ef3947.html](http://hi.baidu.com/victorlin23/blog/item/edc62a35dbae8a1a91ef3947.html)

## 
package com.pccw.portlet.publictools;

 

import java.io.File;

import java.io.FileOutputStream;

import java.io.IOException;

import java.io.InputStream;

import java.io.OutputStream;

import java.io.RandomAccessFile;

import java.net.SocketException;

import java.util.ArrayList;

import java.util.List;

 

import org.apache.commons.logging.Log;

import org.apache.commons.logging.LogFactory;

import org.apache.commons.net.ftp.FTP;

import org.apache.commons.net.ftp.FTPClient;

import org.apache.commons.net.ftp.FTPFile;

import org.apache.commons.net.ftp.FTPReply;

 

/**

 * FTP工具类

 *

 * @author WangXianfeng 1:16:50 PM Jul 16, 2011

 */

public class FtpUtil {

       private FTPClient ftpClient;

       private static Log log = LogFactory.getLog(FtpUtil.class);

      

       /**

        * 根路径为"/",如果需要链接服务器之后跳转到路径，则在path中定义

        * @param ftpConfig

        * @throws SocketException

        * @throws IOException

        */

       public boolean connectServer(FtpConfig ftpConfig) throws SocketException,

                     IOException {

              String server = ftpConfig.getServer();

              int port = ftpConfig.getPort();

              String user = ftpConfig.getUsername();

              String password = ftpConfig.getPassword();

              String path = ftpConfig.getPath();

              return connectServer(server, port, user, password, path);

       }

       /**

        * 连接ftp服务器

        * @param server 服务器ip

        * @param port 端口，通常为21

        * @param user 用户名

        * @param password 密码

        * @param path 进入服务器之后的默认路径

        * @return 连接成功返回true，否则返回false

        * @throws SocketException

        * @throws IOException

        */

       public boolean connectServer(String server, int port, String user,

                     String password, String path) throws SocketException, IOException {

              ftpClient = new FTPClient();

              ftpClient.connect(server, port);

              ftpClient.setControlEncoding("GBK");

              log.info("Connected to " + server + ".");

              log.info("FTP server reply code:" + ftpClient.getReplyCode());

              if (FTPReply.isPositiveCompletion(ftpClient.getReplyCode())) {

                     if (ftpClient.login(user, password)) {

                            // Path is the sub-path of the FTP path

                            if (path.length() != 0) {

                                   ftpClient.changeWorkingDirectory(path);

                            }

                            return true;

                     }

              }

              disconnect();

              return false;

       }

       /**

        * 断开与远程服务器的连接

        * @throws IOException

        */

       public void disconnect() throws IOException {

              if (ftpClient.isConnected()) {

                     ftpClient.disconnect();

              }

       }

      

       /**

        * 从FTP服务器上下载文件,支持断点续传，下载百分比汇报

        * @param remote 远程文件路径及名称

        * @param local 本地文件完整绝对路径

        * @return 下载的状态

        * @throws IOException

        */

       public DownloadStatus download(String remote, String local)

                     throws IOException {

              // 设置被动模式

              ftpClient.enterLocalPassiveMode();

              // 设置以二进制方式传输

              ftpClient.setFileType(FTP.BINARY_FILE_TYPE);

              DownloadStatus result;

 

              // 检查远程文件是否存在

              FTPFile[] files = ftpClient.listFiles(new String(

                            remote.getBytes("GBK"), "iso-8859-1"));

              if (files.length != 1) {

                     log.info("远程文件不存在");

                     return DownloadStatus.RemoteFileNotExist;

              }

 

              long lRemoteSize = files[0].getSize();

              File f = new File(local);

              // 本地存在文件，进行断点下载

              if (f.exists()) {

                     long localSize = f.length();

                     // 判断本地文件大小是否大于远程文件大小

                     if (localSize >= lRemoteSize) {

                            log.info("本地文件大于远程文件，下载中止");

                            return DownloadStatus.LocalFileBiggerThanRemoteFile;

                     }

 

                     // 进行断点续传，并记录状态

                     FileOutputStream out = new FileOutputStream(f, true);

                     ftpClient.setRestartOffset(localSize);

                     InputStream in = ftpClient.retrieveFileStream(new String(remote

                                   .getBytes("GBK"), "iso-8859-1"));

                     byte[] bytes = new byte[1024];

                     long step = lRemoteSize / 100;

                     step = step==0?1:step;//文件过小，step可能为0

                     long process = localSize / step;

                     int c;

                     while ((c = in.read(bytes)) != -1) {

                            out.write(bytes, 0, c);

                            localSize += c;

                            long nowProcess = localSize / step;

                            if (nowProcess > process) {

                                   process = nowProcess;

                                   if (process % 10 == 0){

                                          log.info("下载进度：" + process);

                                   }

                            }

                     }

                     in.close();

                     out.close();

                     boolean isDo = ftpClient.completePendingCommand();

                     if (isDo) {

                            result = DownloadStatus.DownloadFromBreakSuccess;

                     } else {

                            result = DownloadStatus.DownloadFromBreakFailed;

                     }

              } else {

                     OutputStream out = new FileOutputStream(f);

                     InputStream in = ftpClient.retrieveFileStream(new String(remote

                                   .getBytes("GBK"), "iso-8859-1"));

                     byte[] bytes = new byte[1024];

                     long step = lRemoteSize / 100;

                     step = step==0?1:step;//文件过小，step可能为0

                     long process = 0;

                     long localSize = 0L;

                     int c;

                     while ((c = in.read(bytes)) != -1) {

                            out.write(bytes, 0, c);

                            localSize += c;

                            long nowProcess = localSize / step;

                            if (nowProcess > process) {

                                   process = nowProcess;

                                   if (process % 10 == 0){

                                          log.info("下载进度：" + process);

                                   }

                            }

                     }

                     in.close();

                     out.close();

                     boolean upNewStatus = ftpClient.completePendingCommand();

                     if (upNewStatus) {

                            result = DownloadStatus.DownloadNewSuccess;

                     } else {

                            result = DownloadStatus.DownloadNewFailed;

                     }

              }

              return result;

       }

 

       public boolean changeDirectory(String path) throws IOException {

              return ftpClient.changeWorkingDirectory(path);

       }

 

       public boolean createDirectory(String pathName) throws IOException {

              return ftpClient.makeDirectory(pathName);

       }

 

       public boolean removeDirectory(String path) throws IOException {

              return ftpClient.removeDirectory(path);

       }

 

       public boolean removeDirectory(String path, boolean isAll)

                     throws IOException {

 

              if (!isAll) {

                     return removeDirectory(path);

              }

 

              FTPFile[] ftpFileArr = ftpClient.listFiles(path);

              if (ftpFileArr == null || ftpFileArr.length == 0) {

                     return removeDirectory(path);

              }

              //  

              for (FTPFile ftpFile : ftpFileArr) {

                     String name = ftpFile.getName();

                     if (ftpFile.isDirectory()) {

                            log.info("* [sD]Delete subPath [" + path + "/" + name + "]");

                            if (!ftpFile.getName().equals(".")

                                          && (!ftpFile.getName().equals(".."))) {

                                   removeDirectory(path + "/" + name, true);

                            }

                     } else if (ftpFile.isFile()) {

                            log.info("* [sF]Delete file [" + path + "/" + name + "]");

                            deleteFile(path + "/" + name);

                     } else if (ftpFile.isSymbolicLink()) {

 

                     } else if (ftpFile.isUnknown()) {

 

                     }

              }

              return ftpClient.removeDirectory(path);

       }

 

       /**

        * 查看目录是否存在

        * @param path

        * @return

        * @throws IOException

        */

       public boolean isDirectoryExists(String path) throws IOException {

              boolean flag = false;

              FTPFile[] ftpFileArr = ftpClient.listFiles(path);

              for (FTPFile ftpFile : ftpFileArr) {

                     if (ftpFile.isDirectory()

                                   && ftpFile.getName().equalsIgnoreCase(path)) {

                            flag = true;

                            break;

                     }

              }

              return flag;

       }

 

       /**

        * 得到某个目录下的文件名列表

        * @param path

        * @return

        * @throws IOException

        */

       public List<String> getFileList(String path) throws IOException {

              // listFiles return contains directory and file, it's FTPFile instance

              // listNames() contains directory, so using following to filer

              // directory.

              // String[] fileNameArr = ftpClient.listNames(path);

              FTPFile[] ftpFiles = ftpClient.listFiles(path);

 

              List<String> retList = new ArrayList<String>();

              if (ftpFiles == null || ftpFiles.length == 0) {

                     return retList;

              }

              for (FTPFile ftpFile : ftpFiles) {

                     if (ftpFile.isFile()) {

                            retList.add(ftpFile.getName());

                     }

              }

              return retList;

       }

      

       public boolean deleteFile(String pathName) throws IOException {

              return ftpClient.deleteFile(pathName);

       }

 

       /**

        * 上传文件到FTP服务器，支持断点续传

        * @param local 本地文件名称，绝对路径

        * @param remote 远程文件路径，按照Linux上的路径指定方式，支持多级目录嵌套，支持递归创建不存在的目录结构

        * @return 上传结果

        * @throws IOException

        */

       public UploadStatus upload(String local, String remote) throws IOException {

              // 设置PassiveMode传输

              ftpClient.enterLocalPassiveMode();

              // 设置以二进制流的方式传输

              ftpClient.setFileType(FTP.BINARY_FILE_TYPE);

              ftpClient.setControlEncoding("GBK");

              UploadStatus result;

              // 对远程目录的处理

              String remoteFileName = remote;

              if (remote.contains("/")) {

                     remoteFileName = remote.substring(remote.lastIndexOf("/") + 1);

                     // 创建服务器远程目录结构，创建失败直接返回

                     if (createDirecroty(remote, ftpClient) == UploadStatus.CreateDirectoryFail) {

                            return UploadStatus.CreateDirectoryFail;

                     }

              }

 

              // 检查远程是否存在文件

              FTPFile[] files = ftpClient.listFiles(new String(remoteFileName

                            .getBytes("GBK"), "iso-8859-1"));

              if (files.length == 1) {

                     long remoteSize = files[0].getSize();

                     File f = new File(local);

                     long localSize = f.length();

                     if (remoteSize == localSize) { // 文件存在

                            return UploadStatus.FileExits;

                     } else if (remoteSize > localSize) {

                            return UploadStatus.RemoteFileBiggerThanLocalFile;

                     }

 

                     // 尝试移动文件内读取指针,实现断点续传

                     result = uploadFile(remoteFileName, f, ftpClient, remoteSize);

 

                     // 如果断点续传没有成功，则删除服务器上文件，重新上传

                     if (result == UploadStatus.UploadFromBreakFailed) {

                            if (!ftpClient.deleteFile(remoteFileName)) {

                                   return UploadStatus.DeleteRemoteFaild;

                            }

                            result = uploadFile(remoteFileName, f, ftpClient, 0);

                     }

              } else {

                     result = uploadFile(remoteFileName, new File(local), ftpClient, 0);

              }

              return result;

       }

      

       /**

        * 递归创建远程服务器目录

        * @param remote 远程服务器文件绝对路径

        * @param ftpClient FTPClient对象

        * @return 目录创建是否成功

        * @throws IOException

        */

       public UploadStatus createDirecroty(String remote, FTPClient ftpClient)

                     throws IOException {

              UploadStatus status = UploadStatus.CreateDirectorySuccess;

              String directory = remote.substring(0, remote.lastIndexOf("/") + 1);

              if (!directory.equalsIgnoreCase("/")

                            && !ftpClient.changeWorkingDirectory(new String(directory

                                          .getBytes("GBK"), "iso-8859-1"))) {

                     // 如果远程目录不存在，则递归创建远程服务器目录

                     int start = 0;

                     int end = 0;

                     if (directory.startsWith("/")) {

                            start = 1;

                     } else {

                            start = 0;

                     }

                     end = directory.indexOf("/", start);

                     while (true) {

                            String subDirectory = new String(remote.substring(start, end)

                                          .getBytes("GBK"), "iso-8859-1");

                            if (!ftpClient.changeWorkingDirectory(subDirectory)) {

                                   if (ftpClient.makeDirectory(subDirectory)) {

                                          ftpClient.changeWorkingDirectory(subDirectory);

                                   } else {

                                          log.info("创建目录失败");

                                          return UploadStatus.CreateDirectoryFail;

                                   }

                            }

                            start = end + 1;

                            end = directory.indexOf("/", start);

                            // 检查所有目录是否创建完毕

                            if (end <= start) {

                                   break;

                            }

                     }

              }

              return status;

       }

      

       /**

        * 上传文件到服务器,新上传和断点续传

        * @param remoteFile 远程文件名，在上传之前已经将服务器工作目录做了改变

        * @param localFile 本地文件File句柄，绝对路径

        * @param processStep 需要显示的处理进度步进值

        * @param ftpClient FTPClient引用

        * @return

        * @throws IOException

        */

       public UploadStatus uploadFile(String remoteFile, File localFile,

                     FTPClient ftpClient, long remoteSize) throws IOException {

              UploadStatus status;

              // 显示进度的上传

              System.out.println("localFile.length():"+localFile.length());

              long step = localFile.length() / 100;

              step = step==0?1:step;//文件过小，step可能为0

              long process = 0;

              long localreadbytes = 0L;

              RandomAccessFile raf = new RandomAccessFile(localFile, "r");

              OutputStream out = ftpClient.appendFileStream(new String(remoteFile

                            .getBytes("GBK"), "iso-8859-1"));

              // 断点续传

              if (remoteSize > 0) {

                     ftpClient.setRestartOffset(remoteSize);

                     process = remoteSize / step;

                     raf.seek(remoteSize);

                     localreadbytes = remoteSize;

              }

              byte[] bytes = new byte[1024];

              int c;

              while ((c = raf.read(bytes)) != -1) {

                     out.write(bytes, 0, c);

                     localreadbytes += c;

                     if (localreadbytes / step != process) {

                            process = localreadbytes / step;

                            if (process % 10 == 0){

                                   log.info("上传进度：" + process);

                            }

                     }

              }

              out.flush();

              raf.close();

              out.close();

              boolean result = ftpClient.completePendingCommand();

              if (remoteSize > 0) {

                     status = result ? UploadStatus.UploadFromBreakSuccess

                                   : UploadStatus.UploadFromBreakFailed;

              } else {

                     status = result ? UploadStatus.UploadNewFileSuccess

                                   : UploadStatus.UploadNewFileFailed;

              }

              return status;

       }

 

       public InputStream downFile(String sourceFileName) throws IOException {

              return ftpClient.retrieveFileStream(sourceFileName);

       }

      

       public enum UploadStatus {

              CreateDirectoryFail, // 远程服务器相应目录创建失败

              CreateDirectorySuccess, // 远程服务器闯将目录成功

              UploadNewFileSuccess, // 上传新文件成功

              UploadNewFileFailed, // 上传新文件失败

              FileExits, // 文件已经存在

              RemoteFileBiggerThanLocalFile, // 远程文件大于本地文件

              UploadFromBreakSuccess, // 断点续传成功

              UploadFromBreakFailed, // 断点续传失败

              DeleteRemoteFaild; // 删除远程文件失败

       }

 

       public enum DownloadStatus {

              RemoteFileNotExist, // 远程文件不存在

              DownloadNewSuccess, // 下载文件成功

              DownloadNewFailed, // 下载文件失败

              LocalFileBiggerThanRemoteFile, // 本地文件大于远程文件

              DownloadFromBreakSuccess, // 断点续传成功

              DownloadFromBreakFailed; // 断点续传失败

       }

}
FtpConfig类：
/**
 * 
 */
package com.pccw.portlet.publictools;
/**
 * FTP配置类
 * @author WangXianfeng 1:18:45 PM Jul 16, 2011
 */
public class FtpConfig {
    private String server;
    private int port;
    private String username;
    private String password;
    private String path;
    gets  。。sets。。
}
测试类：
import java.util.List;
import com.pccw.portlet.publictools.FtpConfig;
import com.pccw.portlet.publictools.FtpUtil;
/**
 * 
 * @author WangXianfeng 1:20:45 PM Jul 16, 2011
 */
public class HelloWorld {
    /**
     * @param args
     */
    public static void main(String[] args) throws Exception {
        FtpConfig config = new FtpConfig();
        config.setServer("192.168.0.199");
        config.setPort(21);
        config.setUsername("wxf");
        config.setPassword("wxf");
        config.setPath("bankafile");
        FtpUtil ftp = new FtpUtil();
        ftp.connectServer(config);
        String localFile = "c:\\Seagull.rar";
        ftp.upload(localFile, "sea.rar");
        List<String> files = ftp.getFileList("");
        //ftp.changeDirectory("bankafile");
        
        for(String file:files){
            System.out.println(file);
            String local = "D:\\ct\\199\\temp\\" + file;
            ftp.download(file, local);
        }
    }
}

来源： <[http://xianfengmc.blog.163.com/blog/static/82690025201161731615607/](http://xianfengmc.blog.163.com/blog/static/82690025201161731615607/)> 
