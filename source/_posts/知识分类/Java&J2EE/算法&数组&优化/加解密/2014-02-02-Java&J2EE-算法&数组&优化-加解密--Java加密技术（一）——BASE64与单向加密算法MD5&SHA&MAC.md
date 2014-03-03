---
layout: post
title: "Java加密技术（一）——BASE64与单向加密算法MD5&SHA&MAC"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - 算法&数组&优化
 - 加解密
--- 

# Java加密技术（一）——BASE64与单向加密算法MD5&SHA&MAC

    加密解密，曾经是我一个毕业设计的重要组件。在工作了多年以后回想当时那个加密、解密算法，实在是太单纯了。![]()
    言归正传，这里我们主要描述Java已经实现的一些加密解密算法，最后介绍数字证书。
    如基本的单向加密算法：

* BASE64 严格地说，属于编码格式，而非加密算法
* MD5(Message Digest algorithm 5，信息摘要算法)
* SHA(Secure Hash Algorithm，安全散列算法)
* HMAC(Hash Message Authentication Code，散列消息鉴别码)
    复杂的对称加密（DES、PBE）、非对称加密算法：

* DES(Data Encryption Standard，数据加密算法)
* PBE(Password-based encryption，基于密码验证)
* RSA(算法的名字以发明者的名字命名：Ron Rivest, AdiShamir 和Leonard Adleman)
* DH(Diffie-Hellman算法，密钥一致协议)
* DSA(Digital Signature Algorithm，数字签名)
* ECC(Elliptic Curves Cryptography，椭圆曲线密码编码学)
    本篇内容简要介绍**BASE64**、**MD5**、**SHA**、**HMAC**几种方法。
    **MD5**、**SHA**、**HMAC**这三种加密算法，可谓是非可逆加密，就是不可解密的加密方法。我们通常只把他们作为加密的基础。单纯的以上三种的加密并不可靠。
**BASE64**
按照RFC2045的定义，Base64被定义为：Base64内容传送编码被设计用来把任意序列的8位字节描述为一种不易被人直接识别的形式。（The Base64 Content-Transfer-Encoding is designed to represent arbitrary sequences of octets in a form that need not be humanly readable.）
常见于邮件、http加密，截取http信息，你就会发现登录操作的用户名、密码字段通过BASE64加密的。
![]()
通过java代码实现如下：
Java代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. /** 
1.  * BASE64解密 
1.  *  
1.  * @param key 
1.  * @return 
1.  * @throws Exception 
1.  */  
1. public static byte[] decryptBASE64(String key) throws Exception {  
1.     return (new BASE64Decoder()).decodeBuffer(key);  
1. }  
1.   
1. /** 
1.  * BASE64加密 
1.  *  
1.  * @param key 
1.  * @return 
1.  * @throws Exception 
1.  */  
1. public static String encryptBASE64(byte[] key) throws Exception {  
1.     return (new BASE64Encoder()).encodeBuffer(key);  
1. }  

    /**

     * BASE64解密
     *

     * @param key
     * @return

     * @throws Exception
     */

    public static byte[] decryptBASE64(String key) throws Exception {
        return (new BASE64Decoder()).decodeBuffer(key);

    }
 

    /**
     * BASE64加密

     *
     * @param key

     * @return
     * @throws Exception

     */
    public static String encryptBASE64(byte[] key) throws Exception {

        return (new BASE64Encoder()).encodeBuffer(key);
    }
主要就是BASE64Encoder、BASE64Decoder两个类，我们只需要知道使用对应的方法即可。另，BASE加密后产生的字节位数是8的倍数，如果不够位数以**=**符号填充。
**MD5**
MD5 -- message-digest algorithm 5 （信息-摘要算法）缩写，广泛用于加密和解密技术，常用于文件校验。校验？不管文件多大，经过MD5后都能生成唯一的MD5值。好比现在的ISO校验，都是MD5校验。怎么用？当然是把ISO经过MD5后产生MD5的值。一般下载linux-ISO的朋友都见过下载链接旁边放着MD5的串。就是用来验证文件是否一致的。
![]()
通过java代码实现如下：
Java代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. /** 
1.  * MD5加密 
1.  *  
1.  * @param data 
1.  * @return 
1.  * @throws Exception 
1.  */  
1. public static byte[] encryptMD5(byte[] data) throws Exception {  
1.   
1.     MessageDigest md5 = MessageDigest.getInstance(KEY_MD5);  
1.     md5.update(data);  
1.   
1.     return md5.digest();  
1.   
1. }  

    /**

     * MD5加密
     *

     * @param data
     * @return

     * @throws Exception
     */

    public static byte[] encryptMD5(byte[] data) throws Exception {
 

        MessageDigest md5 = MessageDigest.getInstance(KEY_MD5);
        md5.update(data);

 
        return md5.digest();

 
    }
通常我们不直接使用上述MD5加密。通常将MD5产生的字节数组交给BASE64再加密一把，得到相应的字符串。
**SHA**
SHA(Secure Hash Algorithm，安全散列算法），数字签名等密码学应用中重要的工具，被广泛地应用于电子商务等信息安全领域。虽然，SHA与MD5通过碰撞法都被破解了，![]() 但是SHA仍然是公认的安全加密算法，较之MD5更为安全。![]()
![]()
通过java代码实现如下：
Java代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1.     /** 
1.      * SHA加密 
1.      *  
1.      * @param data 
1.      * @return 
1.      * @throws Exception 
1.      */  
1.     public static byte[] encryptSHA(byte[] data) throws Exception {  
1.   
1.         MessageDigest sha = MessageDigest.getInstance(KEY_SHA);  
1.         sha.update(data);  
1.   
1.         return sha.digest();  
1.   
1.     }  
1. }  

    /**

     * SHA加密
     *

     * @param data
     * @return

     * @throws Exception
     */

    public static byte[] encryptSHA(byte[] data) throws Exception {
 

        MessageDigest sha = MessageDigest.getInstance(KEY_SHA);
        sha.update(data);

 
        return sha.digest();

 
    }

}
**HMAC**
HMAC(Hash Message Authentication Code，散列消息鉴别码，基于密钥的Hash算法的认证协议。消息鉴别码实现鉴别的原理是，用公开函数和密钥产生一个固定长度的值作为认证标识，用这个标识鉴别消息的完整性。使用一个密钥生成一个固定大小的小数据块，即MAC，并将其加入到消息中，然后传输。接收方利用与发送方共享的密钥进行鉴别认证等。
![]()
通过java代码实现如下：
Java代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. /** 
1.  * 初始化HMAC密钥 
1.  *  
1.  * @return 
1.  * @throws Exception 
1.  */  
1. public static String initMacKey() throws Exception {  
1.     KeyGenerator keyGenerator = KeyGenerator.getInstance(KEY_MAC);  
1.   
1.     SecretKey secretKey = keyGenerator.generateKey();  
1.     return encryptBASE64(secretKey.getEncoded());  
1. }  
1.   
1. /** 
1.  * HMAC加密 
1.  *  
1.  * @param data 
1.  * @param key 
1.  * @return 
1.  * @throws Exception 
1.  */  
1. public static byte[] encryptHMAC(byte[] data, String key) throws Exception {  
1.   
1.     SecretKey secretKey = new SecretKeySpec(decryptBASE64(key), KEY_MAC);  
1.     Mac mac = Mac.getInstance(secretKey.getAlgorithm());  
1.     mac.init(secretKey);  
1.   
1.     return mac.doFinal(data);  
1.   
1. }  

    /**

     * 初始化HMAC密钥
     *

     * @return
     * @throws Exception

     */
    public static String initMacKey() throws Exception {

        KeyGenerator keyGenerator = KeyGenerator.getInstance(KEY_MAC);
 

        SecretKey secretKey = keyGenerator.generateKey();
        return encryptBASE64(secretKey.getEncoded());

    }
 

    /**
     * HMAC加密

     *
     * @param data

     * @param key
     * @return

     * @throws Exception
     */

    public static byte[] encryptHMAC(byte[] data, String key) throws Exception {
 

        SecretKey secretKey = new SecretKeySpec(decryptBASE64(key), KEY_MAC);
        Mac mac = Mac.getInstance(secretKey.getAlgorithm());

        mac.init(secretKey);
 

        return mac.doFinal(data);
 

    }
给出一个完整类，如下：
Java代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. import java.security.MessageDigest;  
1.   
1. import javax.crypto.KeyGenerator;  
1. import javax.crypto.Mac;  
1. import javax.crypto.SecretKey;  
1.   
1. import sun.misc.BASE64Decoder;  
1. import sun.misc.BASE64Encoder;  
1.   
1. /** 
1.  * 基础加密组件 
1.  *  
1.  * @author 梁栋 
1.  * @version 1.0 
1.  * @since 1.0 
1.  */  
1. public abstract class Coder {  
1.     public static final String KEY_SHA = "SHA";  
1.     public static final String KEY_MD5 = "MD5";  
1.   
1.     /** 
1.      * MAC算法可选以下多种算法 
1.      *  
1.      * <pre> 
1.      * HmacMD5  
1.      * HmacSHA1  
1.      * HmacSHA256  
1.      * HmacSHA384  
1.      * HmacSHA512 
1.      * </pre> 
1.      */  
1.     public static final String KEY_MAC = "HmacMD5";  
1.   
1.     /** 
1.      * BASE64解密 
1.      *  
1.      * @param key 
1.      * @return 
1.      * @throws Exception 
1.      */  
1.     public static byte[] decryptBASE64(String key) throws Exception {  
1.         return (new BASE64Decoder()).decodeBuffer(key);  
1.     }  
1.   
1.     /** 
1.      * BASE64加密 
1.      *  
1.      * @param key 
1.      * @return 
1.      * @throws Exception 
1.      */  
1.     public static String encryptBASE64(byte[] key) throws Exception {  
1.         return (new BASE64Encoder()).encodeBuffer(key);  
1.     }  
1.   
1.     /** 
1.      * MD5加密 
1.      *  
1.      * @param data 
1.      * @return 
1.      * @throws Exception 
1.      */  
1.     public static byte[] encryptMD5(byte[] data) throws Exception {  
1.   
1.         MessageDigest md5 = MessageDigest.getInstance(KEY_MD5);  
1.         md5.update(data);  
1.   
1.         return md5.digest();  
1.   
1.     }  
1.   
1.     /** 
1.      * SHA加密 
1.      *  
1.      * @param data 
1.      * @return 
1.      * @throws Exception 
1.      */  
1.     public static byte[] encryptSHA(byte[] data) throws Exception {  
1.   
1.         MessageDigest sha = MessageDigest.getInstance(KEY_SHA);  
1.         sha.update(data);  
1.   
1.         return sha.digest();  
1.   
1.     }  
1.   
1.     /** 
1.      * 初始化HMAC密钥 
1.      *  
1.      * @return 
1.      * @throws Exception 
1.      */  
1.     public static String initMacKey() throws Exception {  
1.         KeyGenerator keyGenerator = KeyGenerator.getInstance(KEY_MAC);  
1.   
1.         SecretKey secretKey = keyGenerator.generateKey();  
1.         return encryptBASE64(secretKey.getEncoded());  
1.     }  
1.   
1.     /** 
1.      * HMAC加密 
1.      *  
1.      * @param data 
1.      * @param key 
1.      * @return 
1.      * @throws Exception 
1.      */  
1.     public static byte[] encryptHMAC(byte[] data, String key) throws Exception {  
1.   
1.         SecretKey secretKey = new SecretKeySpec(decryptBASE64(key), KEY_MAC);  
1.         Mac mac = Mac.getInstance(secretKey.getAlgorithm());  
1.         mac.init(secretKey);  
1.   
1.         return mac.doFinal(data);  
1.   
1.     }  
1. }  

import java.security.MessageDigest;

 
import javax.crypto.KeyGenerator;

import javax.crypto.Mac;
import javax.crypto.SecretKey;

 
import sun.misc.BASE64Decoder;

import sun.misc.BASE64Encoder;
 

/**
* 基础加密组件

*
* @author 梁栋

* @version 1.0
* @since 1.0

*/
public abstract class Coder {

    public static final String KEY_SHA = "SHA";
    public static final String KEY_MD5 = "MD5";

 
    /**

     * MAC算法可选以下多种算法
     *

     * <pre>
     * HmacMD5

     * HmacSHA1
     * HmacSHA256

     * HmacSHA384
     * HmacSHA512

     * </pre>
     */

    public static final String KEY_MAC = "HmacMD5";
 

    /**
     * BASE64解密

     *
     * @param key

     * @return
     * @throws Exception

     */
    public static byte[] decryptBASE64(String key) throws Exception {

        return (new BASE64Decoder()).decodeBuffer(key);
    }

 
    /**

     * BASE64加密
     *

     * @param key
     * @return

     * @throws Exception
     */

    public static String encryptBASE64(byte[] key) throws Exception {
        return (new BASE64Encoder()).encodeBuffer(key);

    }
 

    /**
     * MD5加密

     *
     * @param data

     * @return
     * @throws Exception

     */
    public static byte[] encryptMD5(byte[] data) throws Exception {

 
        MessageDigest md5 = MessageDigest.getInstance(KEY_MD5);

        md5.update(data);
 

        return md5.digest();
 

    }
 

    /**
     * SHA加密

     *
     * @param data

     * @return
     * @throws Exception

     */
    public static byte[] encryptSHA(byte[] data) throws Exception {

 
        MessageDigest sha = MessageDigest.getInstance(KEY_SHA);

        sha.update(data);
 

        return sha.digest();
 

    }
 

    /**
     * 初始化HMAC密钥

     *
     * @return

     * @throws Exception
     */

    public static String initMacKey() throws Exception {
        KeyGenerator keyGenerator = KeyGenerator.getInstance(KEY_MAC);

 
        SecretKey secretKey = keyGenerator.generateKey();

        return encryptBASE64(secretKey.getEncoded());
    }

 
    /**

     * HMAC加密
     *

     * @param data
     * @param key

     * @return
     * @throws Exception

     */
    public static byte[] encryptHMAC(byte[] data, String key) throws Exception {

 
        SecretKey secretKey = new SecretKeySpec(decryptBASE64(key), KEY_MAC);

        Mac mac = Mac.getInstance(secretKey.getAlgorithm());
        mac.init(secretKey);

 
        return mac.doFinal(data);

 
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
1. public class CoderTest {  
1.   
1.     @Test  
1.     public void test() throws Exception {  
1.         String inputStr = "简单加密";  
1.         System.err.println("原文:\n" + inputStr);  
1.   
1.         byte[] inputData = inputStr.getBytes();  
1.         String code = Coder.encryptBASE64(inputData);  
1.   
1.         System.err.println("BASE64加密后:\n" + code);  
1.   
1.         byte[] output = Coder.decryptBASE64(code);  
1.   
1.         String outputStr = new String(output);  
1.   
1.         System.err.println("BASE64解密后:\n" + outputStr);  
1.   
1.         // 验证BASE64加密解密一致性  
1.         assertEquals(inputStr, outputStr);  
1.   
1.         // 验证MD5对于同一内容加密是否一致  
1.         assertArrayEquals(Coder.encryptMD5(inputData), Coder  
1.                 .encryptMD5(inputData));  
1.   
1.         // 验证SHA对于同一内容加密是否一致  
1.         assertArrayEquals(Coder.encryptSHA(inputData), Coder  
1.                 .encryptSHA(inputData));  
1.   
1.         String key = Coder.initMacKey();  
1.         System.err.println("Mac密钥:\n" + key);  
1.   
1.         // 验证HMAC对于同一内容，同一密钥加密是否一致  
1.         assertArrayEquals(Coder.encryptHMAC(inputData, key), Coder.encryptHMAC(  
1.                 inputData, key));  
1.   
1.         BigInteger md5 = new BigInteger(Coder.encryptMD5(inputData));  
1.         System.err.println("MD5:\n" + md5.toString(16));  
1.   
1.         BigInteger sha = new BigInteger(Coder.encryptSHA(inputData));  
1.         System.err.println("SHA:\n" + sha.toString(32));  
1.   
1.         BigInteger mac = new BigInteger(Coder.encryptHMAC(inputData, inputStr));  
1.         System.err.println("HMAC:\n" + mac.toString(16));  
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
public class CoderTest {

 
    @Test

    public void test() throws Exception {
        String inputStr = "简单加密";

        System.err.println("原文:\n" + inputStr);
 

        byte[] inputData = inputStr.getBytes();
        String code = Coder.encryptBASE64(inputData);

 
        System.err.println("BASE64加密后:\n" + code);

 
        byte[] output = Coder.decryptBASE64(code);

 
        String outputStr = new String(output);

 
        System.err.println("BASE64解密后:\n" + outputStr);

 
        // 验证BASE64加密解密一致性

        assertEquals(inputStr, outputStr);
 

        // 验证MD5对于同一内容加密是否一致
        assertArrayEquals(Coder.encryptMD5(inputData), Coder

                .encryptMD5(inputData));
 

        // 验证SHA对于同一内容加密是否一致
        assertArrayEquals(Coder.encryptSHA(inputData), Coder

                .encryptSHA(inputData));
 

        String key = Coder.initMacKey();
        System.err.println("Mac密钥:\n" + key);

 
        // 验证HMAC对于同一内容，同一密钥加密是否一致

        assertArrayEquals(Coder.encryptHMAC(inputData, key), Coder.encryptHMAC(
                inputData, key));

 
        BigInteger md5 = new BigInteger(Coder.encryptMD5(inputData));

        System.err.println("MD5:\n" + md5.toString(16));
 

        BigInteger sha = new BigInteger(Coder.encryptSHA(inputData));
        System.err.println("SHA:\n" + sha.toString(32));

 
        BigInteger mac = new BigInteger(Coder.encryptHMAC(inputData, inputStr));

        System.err.println("HMAC:\n" + mac.toString(16));
    }

}
控制台输出：
Console代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. 原文:  
1. 简单加密  
1. BASE64加密后:  
1. 566A5Y2V5Yqg5a+G  
1.   
1. BASE64解密后:  
1. 简单加密  
1. Mac密钥:  
1. uGxdHC+6ylRDaik++leFtGwiMbuYUJ6mqHWyhSgF4trVkVBBSQvY/a22xU8XT1RUemdCWW155Bke  
1. pBIpkd7QHg==  
1.   
1. MD5:  
1. -550b4d90349ad4629462113e7934de56  
1. SHA:  
1. 91k9vo7p400cjkgfhjh0ia9qthsjagfn  
1. HMAC:  
1. 2287d192387e95694bdbba2fa941009a  

原文:

简单加密
BASE64加密后:

566A5Y2V5Yqg5a+G
 

BASE64解密后:
简单加密

Mac密钥:
uGxdHC+6ylRDaik++leFtGwiMbuYUJ6mqHWyhSgF4trVkVBBSQvY/a22xU8XT1RUemdCWW155Bke

pBIpkd7QHg==
 

MD5:
-550b4d90349ad4629462113e7934de56

SHA:
91k9vo7p400cjkgfhjh0ia9qthsjagfn

HMAC:
2287d192387e95694bdbba2fa941009a

 
注意
编译时，可能会看到如下提示：
引用

警告：sun.misc.BASE64Decoder 是 Sun 的专用 API，可能会在未来版本中删除
import sun.misc.BASE64Decoder;
               ^
警告：sun.misc.BASE64Encoder 是 Sun 的专用 API，可能会在未来版本中删除
import sun.misc.BASE64Encoder;
               ^
BASE64Encoder和BASE64Decoder是非官方JDK实现类。虽然可以在JDK里能找到并使用，但是在API里查不到。JRE 中 sun 和 com.sun 开头包的类都是未被文档化的，他们属于 java, javax 类库的基础，其中的实现大多数与底层平台有关，一般来说是不推荐使用的。
    BASE64的加密解密是双向的，可以求反解。
    MD5、SHA以及HMAC是单向加密，任何数据加密后只会产生唯一的一个加密串，通常用来校验数据在传输过程中是否被修改。其中HMAC算法有一个密钥，增强了数据传输过程中的安全性，强化了算法外的不可控因素。![]()
    单向加密的用途主要是为了校验数据在传输过程中是否被修改。
**相关链接：
[Java加密技术（一）——BASE64与单向加密算法MD5&SHA&MAC](http://snowolf.iteye.com/blog/379860)
[Java加密技术（二）——对称加密DES&AES](http://snowolf.iteye.com/blog/380034)
[Java加密技术（三）——PBE算法](http://snowolf.iteye.com/blog/380761)
[Java加密技术（四）——非对称加密算法RSA](http://snowolf.iteye.com/blog/381767)
[Java加密技术（五）——非对称加密算法的由来DH](http://snowolf.iteye.com/blog/382422)
[Java加密技术（六）——数字签名算法DSA](http://snowolf.iteye.com/blog/382749)
[Java加密技术（七）——非对称加密算法最高ECC](http://snowolf.iteye.com/blog/383412)
[Java加密技术（八）——数字证书](http://snowolf.iteye.com/blog/391931)
[Java加密技术（九）——初探SSL](http://snowolf.iteye.com/blog/397693)
[Java加密技术（十）——单向认证](http://snowolf.iteye.com/blog/398198)
[Java加密技术（十一）——双向认证](http://snowolf.iteye.com/blog/510985)
[Java加密技术（十二）——*.PFX(*.p12)&个人信息交换文件](http://snowolf.iteye.com/blog/735294)
**
