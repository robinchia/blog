---
layout: post
title: "Java加密技术（八）——数字证书"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - 算法&数组&优化
 - 加解密
--- 

# Java加密技术（八）——数字证书

    本篇的主要内容为Java证书体系的实现。![]()
**请大家在阅读本篇内容时先阅读 [Java加密技术（四）](http://snowolf.iteye.com/blog/381767)，预先了解RSA加密算法。**![]()
在构建Java代码实现前，我们需要完成证书的制作。
1.生成keyStroe文件
在命令行下执行以下命令：
Shell代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. keytool -genkey -validity 36000 -alias www.zlex.org -keyalg RSA -keystore d:\zlex.keystore  

keytool -genkey -validity 36000 -alias www.zlex.org -keyalg RSA -keystore d:\zlex.keystore
其中
**-genkey**表示生成密钥
**-validity**指定证书有效期，这里是**36000**天
**-alias**指定别名，这里是**www.zlex.org**
**-keyalg**指定算法，这里是**RSA**
**-keystore**指定存储位置，这里是**d:\zlex.keystore**
在这里我使用的密码为 **123456**
控制台输出：
Console代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. 输入keystore密码：  
1. 再次输入新密码:  
1. 您的名字与姓氏是什么？  
1.   [Unknown]：  www.zlex.org  
1. 您的组织单位名称是什么？  
1.   [Unknown]：  zlex  
1. 您的组织名称是什么？  
1.   [Unknown]：  zlex  
1. 您所在的城市或区域名称是什么？  
1.   [Unknown]：  BJ  
1. 您所在的州或省份名称是什么？  
1.   [Unknown]：  BJ  
1. 该单位的两字母国家代码是什么  
1.   [Unknown]：  CN  
1. CN=www.zlex.org, OU=zlex, O=zlex, L=BJ, ST=BJ, C=CN 正确吗？  
1.   [否]：  Y  
1.   
1. 输入<tomcat>的主密码  
1.         （如果和 keystore 密码相同，按回车）：  
1. 再次输入新密码:  

输入keystore密码：

再次输入新密码:
您的名字与姓氏是什么？

  [Unknown]：  www.zlex.org
您的组织单位名称是什么？

  [Unknown]：  zlex
您的组织名称是什么？

  [Unknown]：  zlex
您所在的城市或区域名称是什么？

  [Unknown]：  BJ
您所在的州或省份名称是什么？

  [Unknown]：  BJ
该单位的两字母国家代码是什么

  [Unknown]：  CN
CN=www.zlex.org, OU=zlex, O=zlex, L=BJ, ST=BJ, C=CN 正确吗？

  [否]：  Y
 

输入<tomcat>的主密码
        （如果和 keystore 密码相同，按回车）：

再次输入新密码:
 
这时，在D盘下会生成一个zlex.keystore的文件。
2.生成自签名证书
光有keyStore文件是不够的，还需要证书文件，证书才是直接提供给外界使用的公钥凭证。
导出证书：
Shell代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. keytool -export -keystore d:\zlex.keystore -alias www.zlex.org -file d:\zlex.cer -rfc  

keytool -export -keystore d:\zlex.keystore -alias www.zlex.org -file d:\zlex.cer -rfc
其中
**-export**指定为导出操作
**-keystore**指定**keystore文件**
**-alias**指定导出**keystore文件中的别名**
**-file**指向**导出路径**
**-rfc**以文本格式输出，也就是以**BASE64编码**输出
这里的密码是 **123456**
控制台输出：
Console代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. 输入keystore密码：  
1. 保存在文件中的认证 <d:\zlex.cer>  

输入keystore密码：

保存在文件中的认证 <d:\zlex.cer>
当然，使用方是需要导入证书的！
可以通过自签名证书完成CAS单点登录系统的构建！![]()
Ok，准备工作完成，开始Java实现！
通过java代码实现如下：**Coder类见 [Java加密技术（一）](http://snowolf.iteye.com/blog/379860)**
Java代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. import java.io.FileInputStream;  
1. import java.security.KeyStore;  
1. import java.security.PrivateKey;  
1. import java.security.PublicKey;  
1. import java.security.Signature;  
1. import java.security.cert.Certificate;  
1. import java.security.cert.CertificateFactory;  
1. import java.security.cert.X509Certificate;  
1. import java.util.Date;  
1.   
1. import javax.crypto.Cipher;  
1.   
1. /** 
1.  * 证书组件 
1.  *  
1.  * @author 梁栋 
1.  * @version 1.0 
1.  * @since 1.0 
1.  */  
1. public abstract class CertificateCoder extends Coder {  
1.   
1.   
1.     /** 
1.      * Java密钥库(Java Key Store，JKS)KEY_STORE 
1.      */  
1.     public static final String KEY_STORE = "JKS";  
1.   
1.     public static final String X509 = "X.509";  
1.   
1.     /** 
1.      * 由KeyStore获得私钥 
1.      *  
1.      * @param keyStorePath 
1.      * @param alias 
1.      * @param password 
1.      * @return 
1.      * @throws Exception 
1.      */  
1.     private static PrivateKey getPrivateKey(String keyStorePath, String alias,  
1.             String password) throws Exception {  
1.         KeyStore ks = getKeyStore(keyStorePath, password);  
1.         PrivateKey key = (PrivateKey) ks.getKey(alias, password.toCharArray());  
1.         return key;  
1.     }  
1.   
1.     /** 
1.      * 由Certificate获得公钥 
1.      *  
1.      * @param certificatePath 
1.      * @return 
1.      * @throws Exception 
1.      */  
1.     private static PublicKey getPublicKey(String certificatePath)  
1.             throws Exception {  
1.         Certificate certificate = getCertificate(certificatePath);  
1.         PublicKey key = certificate.getPublicKey();  
1.         return key;  
1.     }  
1.   
1.     /** 
1.      * 获得Certificate 
1.      *  
1.      * @param certificatePath 
1.      * @return 
1.      * @throws Exception 
1.      */  
1.     private static Certificate getCertificate(String certificatePath)  
1.             throws Exception {  
1.         CertificateFactory certificateFactory = CertificateFactory  
1.                 .getInstance(X509);  
1.         FileInputStream in = new FileInputStream(certificatePath);  
1.   
1.         Certificate certificate = certificateFactory.generateCertificate(in);  
1.         in.close();  
1.   
1.         return certificate;  
1.     }  
1.   
1.     /** 
1.      * 获得Certificate 
1.      *  
1.      * @param keyStorePath 
1.      * @param alias 
1.      * @param password 
1.      * @return 
1.      * @throws Exception 
1.      */  
1.     private static Certificate getCertificate(String keyStorePath,  
1.             String alias, String password) throws Exception {  
1.         KeyStore ks = getKeyStore(keyStorePath, password);  
1.         Certificate certificate = ks.getCertificate(alias);  
1.   
1.         return certificate;  
1.     }  
1.   
1.     /** 
1.      * 获得KeyStore 
1.      *  
1.      * @param keyStorePath 
1.      * @param password 
1.      * @return 
1.      * @throws Exception 
1.      */  
1.     private static KeyStore getKeyStore(String keyStorePath, String password)  
1.             throws Exception {  
1.         FileInputStream is = new FileInputStream(keyStorePath);  
1.         KeyStore ks = KeyStore.getInstance(KEY_STORE);  
1.         ks.load(is, password.toCharArray());  
1.         is.close();  
1.         return ks;  
1.     }  
1.   
1.     /** 
1.      * 私钥加密 
1.      *  
1.      * @param data 
1.      * @param keyStorePath 
1.      * @param alias 
1.      * @param password 
1.      * @return 
1.      * @throws Exception 
1.      */  
1.     public static byte[] encryptByPrivateKey(byte[] data, String keyStorePath,  
1.             String alias, String password) throws Exception {  
1.         // 取得私钥  
1.         PrivateKey privateKey = getPrivateKey(keyStorePath, alias, password);  
1.   
1.         // 对数据加密  
1.         Cipher cipher = Cipher.getInstance(privateKey.getAlgorithm());  
1.         cipher.init(Cipher.ENCRYPT_MODE, privateKey);  
1.   
1.         return cipher.doFinal(data);  
1.   
1.     }  
1.   
1.     /** 
1.      * 私钥解密 
1.      *  
1.      * @param data 
1.      * @param keyStorePath 
1.      * @param alias 
1.      * @param password 
1.      * @return 
1.      * @throws Exception 
1.      */  
1.     public static byte[] decryptByPrivateKey(byte[] data, String keyStorePath,  
1.             String alias, String password) throws Exception {  
1.         // 取得私钥  
1.         PrivateKey privateKey = getPrivateKey(keyStorePath, alias, password);  
1.   
1.         // 对数据加密  
1.         Cipher cipher = Cipher.getInstance(privateKey.getAlgorithm());  
1.         cipher.init(Cipher.DECRYPT_MODE, privateKey);  
1.   
1.         return cipher.doFinal(data);  
1.   
1.     }  
1.   
1.     /** 
1.      * 公钥加密 
1.      *  
1.      * @param data 
1.      * @param certificatePath 
1.      * @return 
1.      * @throws Exception 
1.      */  
1.     public static byte[] encryptByPublicKey(byte[] data, String certificatePath)  
1.             throws Exception {  
1.   
1.         // 取得公钥  
1.         PublicKey publicKey = getPublicKey(certificatePath);  
1.         // 对数据加密  
1.         Cipher cipher = Cipher.getInstance(publicKey.getAlgorithm());  
1.         cipher.init(Cipher.ENCRYPT_MODE, publicKey);  
1.   
1.         return cipher.doFinal(data);  
1.   
1.     }  
1.   
1.     /** 
1.      * 公钥解密 
1.      *  
1.      * @param data 
1.      * @param certificatePath 
1.      * @return 
1.      * @throws Exception 
1.      */  
1.     public static byte[] decryptByPublicKey(byte[] data, String certificatePath)  
1.             throws Exception {  
1.         // 取得公钥  
1.         PublicKey publicKey = getPublicKey(certificatePath);  
1.   
1.         // 对数据加密  
1.         Cipher cipher = Cipher.getInstance(publicKey.getAlgorithm());  
1.         cipher.init(Cipher.DECRYPT_MODE, publicKey);  
1.   
1.         return cipher.doFinal(data);  
1.   
1.     }  
1.   
1.     /** 
1.      * 验证Certificate 
1.      *  
1.      * @param certificatePath 
1.      * @return 
1.      */  
1.     public static boolean verifyCertificate(String certificatePath) {  
1.         return verifyCertificate(new Date(), certificatePath);  
1.     }  
1.   
1.     /** 
1.      * 验证Certificate是否过期或无效 
1.      *  
1.      * @param date 
1.      * @param certificatePath 
1.      * @return 
1.      */  
1.     public static boolean verifyCertificate(Date date, String certificatePath) {  
1.         boolean status = true;  
1.         try {  
1.             // 取得证书  
1.             Certificate certificate = getCertificate(certificatePath);  
1.             // 验证证书是否过期或无效  
1.             status = verifyCertificate(date, certificate);  
1.         } catch (Exception e) {  
1.             status = false;  
1.         }  
1.         return status;  
1.     }  
1.   
1.     /** 
1.      * 验证证书是否过期或无效 
1.      *  
1.      * @param date 
1.      * @param certificate 
1.      * @return 
1.      */  
1.     private static boolean verifyCertificate(Date date, Certificate certificate) {  
1.         boolean status = true;  
1.         try {  
1.             X509Certificate x509Certificate = (X509Certificate) certificate;  
1.             x509Certificate.checkValidity(date);  
1.         } catch (Exception e) {  
1.             status = false;  
1.         }  
1.         return status;  
1.     }  
1.   
1.     /** 
1.      * 签名 
1.      *  
1.      * @param keyStorePath 
1.      * @param alias 
1.      * @param password 
1.      *  
1.      * @return 
1.      * @throws Exception 
1.      */  
1.     public static String sign(byte[] sign, String keyStorePath, String alias,  
1.             String password) throws Exception {  
1.         // 获得证书  
1.         X509Certificate x509Certificate = (X509Certificate) getCertificate(  
1.                 keyStorePath, alias, password);  
1.         // 获取私钥  
1.         KeyStore ks = getKeyStore(keyStorePath, password);  
1.         // 取得私钥  
1.         PrivateKey privateKey = (PrivateKey) ks.getKey(alias, password  
1.                 .toCharArray());  
1.   
1.         // 构建签名  
1.         Signature signature = Signature.getInstance(x509Certificate  
1.                 .getSigAlgName());  
1.         signature.initSign(privateKey);  
1.         signature.update(sign);  
1.         return encryptBASE64(signature.sign());  
1.     }  
1.   
1.     /** 
1.      * 验证签名 
1.      *  
1.      * @param data 
1.      * @param sign 
1.      * @param certificatePath 
1.      * @return 
1.      * @throws Exception 
1.      */  
1.     public static boolean verify(byte[] data, String sign,  
1.             String certificatePath) throws Exception {  
1.         // 获得证书  
1.         X509Certificate x509Certificate = (X509Certificate) getCertificate(certificatePath);  
1.         // 获得公钥  
1.         PublicKey publicKey = x509Certificate.getPublicKey();  
1.         // 构建签名  
1.         Signature signature = Signature.getInstance(x509Certificate  
1.                 .getSigAlgName());  
1.         signature.initVerify(publicKey);  
1.         signature.update(data);  
1.   
1.         return signature.verify(decryptBASE64(sign));  
1.   
1.     }  
1.   
1.     /** 
1.      * 验证Certificate 
1.      *  
1.      * @param keyStorePath 
1.      * @param alias 
1.      * @param password 
1.      * @return 
1.      */  
1.     public static boolean verifyCertificate(Date date, String keyStorePath,  
1.             String alias, String password) {  
1.         boolean status = true;  
1.         try {  
1.             Certificate certificate = getCertificate(keyStorePath, alias,  
1.                     password);  
1.             status = verifyCertificate(date, certificate);  
1.         } catch (Exception e) {  
1.             status = false;  
1.         }  
1.         return status;  
1.     }  
1.   
1.     /** 
1.      * 验证Certificate 
1.      *  
1.      * @param keyStorePath 
1.      * @param alias 
1.      * @param password 
1.      * @return 
1.      */  
1.     public static boolean verifyCertificate(String keyStorePath, String alias,  
1.             String password) {  
1.         return verifyCertificate(new Date(), keyStorePath, alias, password);  
1.     }  
1. }  

import java.io.FileInputStream;

import java.security.KeyStore;
import java.security.PrivateKey;

import java.security.PublicKey;
import java.security.Signature;

import java.security.cert.Certificate;
import java.security.cert.CertificateFactory;

import java.security.cert.X509Certificate;
import java.util.Date;

 
import javax.crypto.Cipher;

 
/**

* 证书组件
*

* @author 梁栋
* @version 1.0

* @since 1.0
*/

public abstract class CertificateCoder extends Coder {
 

 
    /**

     * Java密钥库(Java Key Store，JKS)KEY_STORE
     */

    public static final String KEY_STORE = "JKS";
 

    public static final String X509 = "X.509";
 

    /**
     * 由KeyStore获得私钥

     *
     * @param keyStorePath

     * @param alias
     * @param password

     * @return
     * @throws Exception

     */
    private static PrivateKey getPrivateKey(String keyStorePath, String alias,

            String password) throws Exception {
        KeyStore ks = getKeyStore(keyStorePath, password);

        PrivateKey key = (PrivateKey) ks.getKey(alias, password.toCharArray());
        return key;

    }
 

    /**
     * 由Certificate获得公钥

     *
     * @param certificatePath

     * @return
     * @throws Exception

     */
    private static PublicKey getPublicKey(String certificatePath)

            throws Exception {
        Certificate certificate = getCertificate(certificatePath);

        PublicKey key = certificate.getPublicKey();
        return key;

    }
 

    /**
     * 获得Certificate

     *
     * @param certificatePath

     * @return
     * @throws Exception

     */
    private static Certificate getCertificate(String certificatePath)

            throws Exception {
        CertificateFactory certificateFactory = CertificateFactory

                .getInstance(X509);
        FileInputStream in = new FileInputStream(certificatePath);

 
        Certificate certificate = certificateFactory.generateCertificate(in);

        in.close();
 

        return certificate;
    }

 
    /**

     * 获得Certificate
     *

     * @param keyStorePath
     * @param alias

     * @param password
     * @return

     * @throws Exception
     */

    private static Certificate getCertificate(String keyStorePath,
            String alias, String password) throws Exception {

        KeyStore ks = getKeyStore(keyStorePath, password);
        Certificate certificate = ks.getCertificate(alias);

 
        return certificate;

    }
 

    /**
     * 获得KeyStore

     *
     * @param keyStorePath

     * @param password
     * @return

     * @throws Exception
     */

    private static KeyStore getKeyStore(String keyStorePath, String password)
            throws Exception {

        FileInputStream is = new FileInputStream(keyStorePath);
        KeyStore ks = KeyStore.getInstance(KEY_STORE);

        ks.load(is, password.toCharArray());
        is.close();

        return ks;
    }

 
    /**

     * 私钥加密
     *

     * @param data
     * @param keyStorePath

     * @param alias
     * @param password

     * @return
     * @throws Exception

     */
    public static byte[] encryptByPrivateKey(byte[] data, String keyStorePath,

            String alias, String password) throws Exception {
        // 取得私钥

        PrivateKey privateKey = getPrivateKey(keyStorePath, alias, password);
 

        // 对数据加密
        Cipher cipher = Cipher.getInstance(privateKey.getAlgorithm());

        cipher.init(Cipher.ENCRYPT_MODE, privateKey);
 

        return cipher.doFinal(data);
 

    }
 

    /**
     * 私钥解密

     *
     * @param data

     * @param keyStorePath
     * @param alias

     * @param password
     * @return

     * @throws Exception
     */

    public static byte[] decryptByPrivateKey(byte[] data, String keyStorePath,
            String alias, String password) throws Exception {

        // 取得私钥
        PrivateKey privateKey = getPrivateKey(keyStorePath, alias, password);

 
        // 对数据加密

        Cipher cipher = Cipher.getInstance(privateKey.getAlgorithm());
        cipher.init(Cipher.DECRYPT_MODE, privateKey);

 
        return cipher.doFinal(data);

 
    }

 
    /**

     * 公钥加密
     *

     * @param data
     * @param certificatePath

     * @return
     * @throws Exception

     */
    public static byte[] encryptByPublicKey(byte[] data, String certificatePath)

            throws Exception {
 

        // 取得公钥
        PublicKey publicKey = getPublicKey(certificatePath);

        // 对数据加密
        Cipher cipher = Cipher.getInstance(publicKey.getAlgorithm());

        cipher.init(Cipher.ENCRYPT_MODE, publicKey);
 

        return cipher.doFinal(data);
 

    }
 

    /**
     * 公钥解密

     *
     * @param data

     * @param certificatePath
     * @return

     * @throws Exception
     */

    public static byte[] decryptByPublicKey(byte[] data, String certificatePath)
            throws Exception {

        // 取得公钥
        PublicKey publicKey = getPublicKey(certificatePath);

 
        // 对数据加密

        Cipher cipher = Cipher.getInstance(publicKey.getAlgorithm());
        cipher.init(Cipher.DECRYPT_MODE, publicKey);

 
        return cipher.doFinal(data);

 
    }

 
    /**

     * 验证Certificate
     *

     * @param certificatePath
     * @return

     */
    public static boolean verifyCertificate(String certificatePath) {

        return verifyCertificate(new Date(), certificatePath);
    }

 
    /**

     * 验证Certificate是否过期或无效
     *

     * @param date
     * @param certificatePath

     * @return
     */

    public static boolean verifyCertificate(Date date, String certificatePath) {
        boolean status = true;

        try {
            // 取得证书

            Certificate certificate = getCertificate(certificatePath);
            // 验证证书是否过期或无效

            status = verifyCertificate(date, certificate);
        } catch (Exception e) {

            status = false;
        }

        return status;
    }

 
    /**

     * 验证证书是否过期或无效
     *

     * @param date
     * @param certificate

     * @return
     */

    private static boolean verifyCertificate(Date date, Certificate certificate) {
        boolean status = true;

        try {
            X509Certificate x509Certificate = (X509Certificate) certificate;

            x509Certificate.checkValidity(date);
        } catch (Exception e) {

            status = false;
        }

        return status;
    }

 
    /**

     * 签名
     *

     * @param keyStorePath
     * @param alias

     * @param password
     *

     * @return
     * @throws Exception

     */
    public static String sign(byte[] sign, String keyStorePath, String alias,

            String password) throws Exception {
        // 获得证书

        X509Certificate x509Certificate = (X509Certificate) getCertificate(
                keyStorePath, alias, password);

        // 获取私钥
        KeyStore ks = getKeyStore(keyStorePath, password);

        // 取得私钥
        PrivateKey privateKey = (PrivateKey) ks.getKey(alias, password

                .toCharArray());
 

        // 构建签名
        Signature signature = Signature.getInstance(x509Certificate

                .getSigAlgName());
        signature.initSign(privateKey);

        signature.update(sign);
        return encryptBASE64(signature.sign());

    }
 

    /**
     * 验证签名

     *
     * @param data

     * @param sign
     * @param certificatePath

     * @return
     * @throws Exception

     */
    public static boolean verify(byte[] data, String sign,

            String certificatePath) throws Exception {
        // 获得证书

        X509Certificate x509Certificate = (X509Certificate) getCertificate(certificatePath);
        // 获得公钥

        PublicKey publicKey = x509Certificate.getPublicKey();
        // 构建签名

        Signature signature = Signature.getInstance(x509Certificate
                .getSigAlgName());

        signature.initVerify(publicKey);
        signature.update(data);

 
        return signature.verify(decryptBASE64(sign));

 
    }

 
    /**

     * 验证Certificate
     *

     * @param keyStorePath
     * @param alias

     * @param password
     * @return

     */
    public static boolean verifyCertificate(Date date, String keyStorePath,

            String alias, String password) {
        boolean status = true;

        try {
            Certificate certificate = getCertificate(keyStorePath, alias,

                    password);
            status = verifyCertificate(date, certificate);

        } catch (Exception e) {
            status = false;

        }
        return status;

    }
 

    /**
     * 验证Certificate

     *
     * @param keyStorePath

     * @param alias
     * @param password

     * @return
     */

    public static boolean verifyCertificate(String keyStorePath, String alias,
            String password) {

        return verifyCertificate(new Date(), keyStorePath, alias, password);
    }

}
再给出一个测试类：
Java代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. import static org.junit.Assert.*;  
1.   
1. import org.junit.Test;  
1.   
1. /** 
1.  *  
1.  * @author 梁栋 
1.  * @version 1.0 
1.  * @since 1.0 
1.  */  
1. public class CertificateCoderTest {  
1.     private String password = "123456";  
1.     private String alias = "www.zlex.org";  
1.     private String certificatePath = "d:/zlex.cer";  
1.     private String keyStorePath = "d:/zlex.keystore";  
1.   
1.     @Test  
1.     public void test() throws Exception {  
1.         System.err.println("公钥加密——私钥解密");  
1.         String inputStr = "Ceritifcate";  
1.         byte[] data = inputStr.getBytes();  
1.   
1.         byte[] encrypt = CertificateCoder.encryptByPublicKey(data,  
1.                 certificatePath);  
1.   
1.         byte[] decrypt = CertificateCoder.decryptByPrivateKey(encrypt,  
1.                 keyStorePath, alias, password);  
1.         String outputStr = new String(decrypt);  
1.   
1.         System.err.println("加密前: " + inputStr + "\n\r" + "解密后: " + outputStr);  
1.   
1.         // 验证数据一致  
1.         assertArrayEquals(data, decrypt);  
1.   
1.         // 验证证书有效  
1.         assertTrue(CertificateCoder.verifyCertificate(certificatePath));  
1.   
1.     }  
1.   
1.     @Test  
1.     public void testSign() throws Exception {  
1.         System.err.println("私钥加密——公钥解密");  
1.   
1.         String inputStr = "sign";  
1.         byte[] data = inputStr.getBytes();  
1.   
1.         byte[] encodedData = CertificateCoder.encryptByPrivateKey(data,  
1.                 keyStorePath, alias, password);  
1.   
1.         byte[] decodedData = CertificateCoder.decryptByPublicKey(encodedData,  
1.                 certificatePath);  
1.   
1.         String outputStr = new String(decodedData);  
1.         System.err.println("加密前: " + inputStr + "\n\r" + "解密后: " + outputStr);  
1.         assertEquals(inputStr, outputStr);  
1.   
1.         System.err.println("私钥签名——公钥验证签名");  
1.         // 产生签名  
1.         String sign = CertificateCoder.sign(encodedData, keyStorePath, alias,  
1.                 password);  
1.         System.err.println("签名:\r" + sign);  
1.   
1.         // 验证签名  
1.         boolean status = CertificateCoder.verify(encodedData, sign,  
1.                 certificatePath);  
1.         System.err.println("状态:\r" + status);  
1.         assertTrue(status);  
1.   
1.     }  
1. }  

import static org.junit.Assert.*;

 
import org.junit.Test;

 
/**

*
* @author 梁栋

* @version 1.0
* @since 1.0

*/
public class CertificateCoderTest {

    private String password = "123456";
    private String alias = "www.zlex.org";

    private String certificatePath = "d:/zlex.cer";
    private String keyStorePath = "d:/zlex.keystore";

 
    @Test

    public void test() throws Exception {
        System.err.println("公钥加密——私钥解密");

        String inputStr = "Ceritifcate";
        byte[] data = inputStr.getBytes();

 
        byte[] encrypt = CertificateCoder.encryptByPublicKey(data,

                certificatePath);
 

        byte[] decrypt = CertificateCoder.decryptByPrivateKey(encrypt,
                keyStorePath, alias, password);

        String outputStr = new String(decrypt);
 

        System.err.println("加密前: " + inputStr + "\n\r" + "解密后: " + outputStr);
 

        // 验证数据一致
        assertArrayEquals(data, decrypt);

 
        // 验证证书有效

        assertTrue(CertificateCoder.verifyCertificate(certificatePath));
 

    }
 

    @Test
    public void testSign() throws Exception {

        System.err.println("私钥加密——公钥解密");
 

        String inputStr = "sign";
        byte[] data = inputStr.getBytes();

 
        byte[] encodedData = CertificateCoder.encryptByPrivateKey(data,

                keyStorePath, alias, password);
 

        byte[] decodedData = CertificateCoder.decryptByPublicKey(encodedData,
                certificatePath);

 
        String outputStr = new String(decodedData);

        System.err.println("加密前: " + inputStr + "\n\r" + "解密后: " + outputStr);
        assertEquals(inputStr, outputStr);

 
        System.err.println("私钥签名——公钥验证签名");

        // 产生签名
        String sign = CertificateCoder.sign(encodedData, keyStorePath, alias,

                password);
        System.err.println("签名:\r" + sign);

 
        // 验证签名

        boolean status = CertificateCoder.verify(encodedData, sign,
                certificatePath);

        System.err.println("状态:\r" + status);
        assertTrue(status);

 
    }

}
控制台输出：
Console代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. 公钥加密——私钥解密  
1. 加密前: Ceritificate  
1.   
1. 解密后: Ceritificate  
1.   
1. 私钥加密——公钥解密  
1. 加密前: sign  
1.   
1. 解密后: sign  
1. 私钥签名——公钥验证签名  
1. 签名:  
1. pqBn5m6PJlfOjH0A6U2o2mUmBsfgyEY1NWCbiyA/I5Gc3gaVNVIdj/zkGNZRqTjhf3+J9a9z9EI7  
1. 6F2eWYd7punHx5oh6hfNgcKbVb52EfItl4QEN+djbXiPynn07+Lbg1NOjULnpEd6ZhLP1YwrEAuM  
1. OfvX0e7/wplxLbySaKQ=  
1.   
1. 状态:  
1. true  

公钥加密——私钥解密

加密前: Ceritificate
 

解密后: Ceritificate
 

私钥加密——公钥解密
加密前: sign

 
解密后: sign

私钥签名——公钥验证签名
签名:

pqBn5m6PJlfOjH0A6U2o2mUmBsfgyEY1NWCbiyA/I5Gc3gaVNVIdj/zkGNZRqTjhf3+J9a9z9EI7
6F2eWYd7punHx5oh6hfNgcKbVb52EfItl4QEN+djbXiPynn07+Lbg1NOjULnpEd6ZhLP1YwrEAuM

OfvX0e7/wplxLbySaKQ=
 

状态:
true
由此完成了证书验证体系！![]()
同样，我们可以对代码做签名——代码签名！![]()
通过工具JarSigner可以完成代码签名。
这里我们对tools.jar做代码签名，命令如下：
Shell代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. jarsigner -storetype jks -keystore zlex.keystore -verbose tools.jar www.zlex.org  

jarsigner -storetype jks -keystore zlex.keystore -verbose tools.jar www.zlex.org
控制台输出：
Console代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. 输入密钥库的口令短语：  
1.  正在更新： META-INF/WWW_ZLEX.SF  
1.  正在更新： META-INF/WWW_ZLEX.RSA  
1.   正在签名： org/zlex/security/Security.class  
1.   正在签名： org/zlex/tool/Main$1.class  
1.   正在签名： org/zlex/tool/Main$2.class  
1.   正在签名： org/zlex/tool/Main.class  
1.   
1. 警告：  
1. 签名者证书将在六个月内过期。  

输入密钥库的口令短语：

正在更新： META-INF/WWW_ZLEX.SF
正在更新： META-INF/WWW_ZLEX.RSA

  正在签名： org/zlex/security/Security.class
  正在签名： org/zlex/tool/Main$1.class

  正在签名： org/zlex/tool/Main$2.class
  正在签名： org/zlex/tool/Main.class

 
警告：

签名者证书将在六个月内过期。
此时，我们可以对签名后的jar做验证！![]()
验证tools.jar，命令如下：
Shell代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. jarsigner -verify -verbose -certs tools.jar  

jarsigner -verify -verbose -certs tools.jar
控制台输出：
Console代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1.          402 Sat Jun 20 16:25:14 CST 2009 META-INF/MANIFEST.MF  
1.          532 Sat Jun 20 16:25:14 CST 2009 META-INF/WWW_ZLEX.SF  
1.          889 Sat Jun 20 16:25:14 CST 2009 META-INF/WWW_ZLEX.RSA  
1. sm       590 Wed Dec 10 13:03:42 CST 2008 org/zlex/security/Security.class  
1.   
1.       X.509, CN=www.zlex.org, OU=zlex, O=zlex, L=BJ, ST=BJ, C=CN  
1.       [证书将在 09-9-18 下午3:27 到期]  
1.   
1. sm       705 Tue Dec 16 18:00:56 CST 2008 org/zlex/tool/Main$1.class  
1.   
1.       X.509, CN=www.zlex.org, OU=zlex, O=zlex, L=BJ, ST=BJ, C=CN  
1.       [证书将在 09-9-18 下午3:27 到期]  
1.   
1. sm       779 Tue Dec 16 18:00:56 CST 2008 org/zlex/tool/Main$2.class  
1.   
1.       X.509, CN=www.zlex.org, OU=zlex, O=zlex, L=BJ, ST=BJ, C=CN  
1.       [证书将在 09-9-18 下午3:27 到期]  
1.   
1. sm     12672 Tue Dec 16 18:00:56 CST 2008 org/zlex/tool/Main.class  
1.   
1.       X.509, CN=www.zlex.org, OU=zlex, O=zlex, L=BJ, ST=BJ, C=CN  
1.       [证书将在 09-9-18 下午3:27 到期]  
1.   
1.   
1.   s = 已验证签名  
1.   m = 在清单中列出条目  
1.   k = 在密钥库中至少找到了一个证书  
1.   i = 在身份作用域内至少找到了一个证书  
1.   
1. jar 已验证。  
1.   
1. 警告：  
1. 此 jar 包含签名者证书将在六个月内过期的条目。  

         402 Sat Jun 20 16:25:14 CST 2009 META-INF/MANIFEST.MF

         532 Sat Jun 20 16:25:14 CST 2009 META-INF/WWW_ZLEX.SF
         889 Sat Jun 20 16:25:14 CST 2009 META-INF/WWW_ZLEX.RSA

sm       590 Wed Dec 10 13:03:42 CST 2008 org/zlex/security/Security.class
 

      X.509, CN=www.zlex.org, OU=zlex, O=zlex, L=BJ, ST=BJ, C=CN
      [证书将在 09-9-18 下午3:27 到期]

 
sm       705 Tue Dec 16 18:00:56 CST 2008 org/zlex/tool/Main$1.class

 
      X.509, CN=www.zlex.org, OU=zlex, O=zlex, L=BJ, ST=BJ, C=CN

      [证书将在 09-9-18 下午3:27 到期]
 

sm       779 Tue Dec 16 18:00:56 CST 2008 org/zlex/tool/Main$2.class
 

      X.509, CN=www.zlex.org, OU=zlex, O=zlex, L=BJ, ST=BJ, C=CN
      [证书将在 09-9-18 下午3:27 到期]

 
sm     12672 Tue Dec 16 18:00:56 CST 2008 org/zlex/tool/Main.class

 
      X.509, CN=www.zlex.org, OU=zlex, O=zlex, L=BJ, ST=BJ, C=CN

      [证书将在 09-9-18 下午3:27 到期]
 

 
  s = 已验证签名

  m = 在清单中列出条目
  k = 在密钥库中至少找到了一个证书

  i = 在身份作用域内至少找到了一个证书
 

jar 已验证。
 

警告：
此 jar 包含签名者证书将在六个月内过期的条目。
代码签名认证的用途主要是对发布的软件做验证，支持 Sun Java .jar (Java Applet) 文件(J2SE)和 J2ME MIDlet Suite 文件。
![]()
**相关链接：
[Java加密技术（一）——BASE64与单向加密算法MD5&SHA&MAC](http://snowolf.iteye.com/blog/379860)
[Java加密技术（二）——对称加密DES&AES](http://snowolf.iteye.com/blog/380034)
[Java加密技术（三）——PBE算法](http://snowolf.iteye.com/blog/380761)
[Java加密技术（四）——非对称加密算法RSA](http://snowolf.iteye.com/blog/381767)
[Java加密技术（五）——非对称加密算法的由来](http://snowolf.iteye.com/blog/382422)
[Java加密技术（六）——数字签名算法DSA](http://snowolf.iteye.com/blog/382749)
[Java加密技术（七）——非对称加密算法最高ECC](http://snowolf.iteye.com/blog/383412)
[Java加密技术（八）——数字证书](http://snowolf.iteye.com/blog/391931)
[Java加密技术（九）——初探SSL](http://snowolf.iteye.com/blog/397693)
[Java加密技术（十）——单向认证](http://snowolf.iteye.com/blog/398198)
[Java加密技术（十一）——双向认证](http://snowolf.iteye.com/blog/510985)
[Java加密技术（十二）——*.PFX(*.p12)&个人信息交换文件](http://snowolf.iteye.com/blog/735294)
**
