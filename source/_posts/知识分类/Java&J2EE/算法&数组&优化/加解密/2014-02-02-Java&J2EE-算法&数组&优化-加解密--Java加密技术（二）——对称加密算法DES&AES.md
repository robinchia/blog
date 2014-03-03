---
layout: post
title: "Java加密技术（二）——对称加密算法DES&AES"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - 算法&数组&优化
 - 加解密
--- 

# Java加密技术（二）——对称加密算法DES&AES

    接下来我们介绍对称加密算法，最常用的莫过于DES数据加密算法。
**DES**
DES-Data Encryption Standard,即数据加密算法。是IBM公司于1975年研究成功并公开发表的。DES算法的入口参数有三个:Key、Data、Mode。其中Key为8个字节共64位,是DES算法的工作密钥;Data也为8个字节64位,是要被加密或被解密的数据;Mode为DES的工作方式,有两种:加密或解密。
DES算法把64位的明文输入块变为64位的密文输出块,它所使用的密钥也是64位。
![]()
通过java代码实现如下：**Coder类见 [Java加密技术（一）](http://snowolf.iteye.com/blog/379860)**
Java代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. import java.security.Key;  
1. import java.security.SecureRandom;  
1.   
1. import javax.crypto.Cipher;  
1. import javax.crypto.KeyGenerator;  
1. import javax.crypto.SecretKey;  
1. import javax.crypto.SecretKeyFactory;  
1. import javax.crypto.spec.DESKeySpec;  
1.   
1.   
1. /** 
1.  * DES安全编码组件 
1.  *  
1.  * <pre> 
1.  * 支持 DES、DESede(TripleDES,就是3DES)、AES、Blowfish、RC2、RC4(ARCFOUR) 
1.  * DES                  key size must be equal to 56 
1.  * DESede(TripleDES)    key size must be equal to 112 or 168 
1.  * AES                  key size must be equal to 128, 192 or 256,but 192 and 256 bits may not be available 
1.  * Blowfish             key size must be multiple of 8, and can only range from 32 to 448 (inclusive) 
1.  * RC2                  key size must be between 40 and 1024 bits 
1.  * RC4(ARCFOUR)         key size must be between 40 and 1024 bits 
1.  * 具体内容 需要关注 JDK Document http://.../docs/technotes/guides/security/SunProviders.html 
1.  * </pre> 
1.  *  
1.  * @author 梁栋 
1.  * @version 1.0 
1.  * @since 1.0 
1.  */  
1. public abstract class DESCoder extends Coder {  
1.     /** 
1.      * ALGORITHM 算法 <br> 
1.      * 可替换为以下任意一种算法，同时key值的size相应改变。 
1.      *  
1.      * <pre> 
1.      * DES                  key size must be equal to 56 
1.      * DESede(TripleDES)    key size must be equal to 112 or 168 
1.      * AES                  key size must be equal to 128, 192 or 256,but 192 and 256 bits may not be available 
1.      * Blowfish             key size must be multiple of 8, and can only range from 32 to 448 (inclusive) 
1.      * RC2                  key size must be between 40 and 1024 bits 
1.      * RC4(ARCFOUR)         key size must be between 40 and 1024 bits 
1.      * </pre> 
1.      *  
1.      * 在Key toKey(byte[] key)方法中使用下述代码 
1.      * <code>SecretKey secretKey = new SecretKeySpec(key, ALGORITHM);</code> 替换 
1.  
1.      * <code> 
1.      * DESKeySpec dks = new DESKeySpec(key); 
1.      * SecretKeyFactory keyFactory = SecretKeyFactory.getInstance(ALGORITHM); 
1.      * SecretKey secretKey = keyFactory.generateSecret(dks); 
1.      * </code> 
1.      */  
1.     public static final String ALGORITHM = "DES";  
1.   
1.     /** 
1.      * 转换密钥<br> 
1.      *  
1.      * @param key 
1.      * @return 
1.      * @throws Exception 
1.      */  
1.     private static Key toKey(byte[] key) throws Exception {  
1.         DESKeySpec dks = new DESKeySpec(key);  
1.         SecretKeyFactory keyFactory = SecretKeyFactory.getInstance(ALGORITHM);  
1.         SecretKey secretKey = keyFactory.generateSecret(dks);  
1.   
1.         // 当使用其他对称加密算法时，如AES、Blowfish等算法时，用下述代码替换上述三行代码  
1.         // SecretKey secretKey = new SecretKeySpec(key, ALGORITHM);  
1.   
1.         return secretKey;  
1.     }  
1.   
1.     /** 
1.      * 解密 
1.      *  
1.      * @param data 
1.      * @param key 
1.      * @return 
1.      * @throws Exception 
1.      */  
1.     public static byte[] decrypt(byte[] data, String key) throws Exception {  
1.         Key k = toKey(decryptBASE64(key));  
1.   
1.         Cipher cipher = Cipher.getInstance(ALGORITHM);  
1.         cipher.init(Cipher.DECRYPT_MODE, k);  
1.   
1.         return cipher.doFinal(data);  
1.     }  
1.   
1.     /** 
1.      * 加密 
1.      *  
1.      * @param data 
1.      * @param key 
1.      * @return 
1.      * @throws Exception 
1.      */  
1.     public static byte[] encrypt(byte[] data, String key) throws Exception {  
1.         Key k = toKey(decryptBASE64(key));  
1.         Cipher cipher = Cipher.getInstance(ALGORITHM);  
1.         cipher.init(Cipher.ENCRYPT_MODE, k);  
1.   
1.         return cipher.doFinal(data);  
1.     }  
1.   
1.     /** 
1.      * 生成密钥 
1.      *  
1.      * @return 
1.      * @throws Exception 
1.      */  
1.     public static String initKey() throws Exception {  
1.         return initKey(null);  
1.     }  
1.   
1.     /** 
1.      * 生成密钥 
1.      *  
1.      * @param seed 
1.      * @return 
1.      * @throws Exception 
1.      */  
1.     public static String initKey(String seed) throws Exception {  
1.         SecureRandom secureRandom = null;  
1.   
1.         if (seed != null) {  
1.             secureRandom = new SecureRandom(decryptBASE64(seed));  
1.         } else {  
1.             secureRandom = new SecureRandom();  
1.         }  
1.   
1.         KeyGenerator kg = KeyGenerator.getInstance(ALGORITHM);  
1.         kg.init(secureRandom);  
1.   
1.         SecretKey secretKey = kg.generateKey();  
1.   
1.         return encryptBASE64(secretKey.getEncoded());  
1.     }  
1. }  

import java.security.Key;

import java.security.SecureRandom;
 

import javax.crypto.Cipher;
import javax.crypto.KeyGenerator;

import javax.crypto.SecretKey;
import javax.crypto.SecretKeyFactory;

import javax.crypto.spec.DESKeySpec;
 

 
/**

* DES安全编码组件
*

* <pre>
* 支持 DES、DESede(TripleDES,就是3DES)、AES、Blowfish、RC2、RC4(ARCFOUR)

 * DES                  key size must be equal to 56
 * DESede(TripleDES)     key size must be equal to 112 or 168

 * AES                  key size must be equal to 128, 192 or 256,but 192 and 256 bits may not be available
 * Blowfish             key size must be multiple of 8, and can only range from 32 to 448 (inclusive)

 * RC2                  key size must be between 40 and 1024 bits
 * RC4(ARCFOUR)         key size must be between 40 and 1024 bits

* 具体内容 需要关注 JDK Document http://.../docs/technotes/guides/security/SunProviders.html
* </pre>

*
* @author 梁栋

* @version 1.0
* @since 1.0

*/
public abstract class DESCoder extends Coder {

    /**
     * ALGORITHM 算法 <br>

     * 可替换为以下任意一种算法，同时key值的size相应改变。
     *

     * <pre>
     * DES                  key size must be equal to 56

     * DESede(TripleDES)     key size must be equal to 112 or 168
     * AES                  key size must be equal to 128, 192 or 256,but 192 and 256 bits may not be available

     * Blowfish             key size must be multiple of 8, and can only range from 32 to 448 (inclusive)
     * RC2                  key size must be between 40 and 1024 bits

     * RC4(ARCFOUR)         key size must be between 40 and 1024 bits
     * </pre>

     *
     * 在Key toKey(byte[] key)方法中使用下述代码

     * <code>SecretKey secretKey = new SecretKeySpec(key, ALGORITHM);</code> 替换
 

     * <code>
     * DESKeySpec dks = new DESKeySpec(key);

     * SecretKeyFactory keyFactory = SecretKeyFactory.getInstance(ALGORITHM);
     * SecretKey secretKey = keyFactory.generateSecret(dks);

     * </code>
     */

    public static final String ALGORITHM = "DES";
 

    /**
     * 转换密钥<br>

     *
     * @param key

     * @return
     * @throws Exception

     */
    private static Key toKey(byte[] key) throws Exception {

        DESKeySpec dks = new DESKeySpec(key);
        SecretKeyFactory keyFactory = SecretKeyFactory.getInstance(ALGORITHM);

        SecretKey secretKey = keyFactory.generateSecret(dks);
 

        // 当使用其他对称加密算法时，如AES、Blowfish等算法时，用下述代码替换上述三行代码
        // SecretKey secretKey = new SecretKeySpec(key, ALGORITHM);

 
        return secretKey;

    }
 

    /**
     * 解密

     *
     * @param data

     * @param key
     * @return

     * @throws Exception
     */

    public static byte[] decrypt(byte[] data, String key) throws Exception {
        Key k = toKey(decryptBASE64(key));

 
        Cipher cipher = Cipher.getInstance(ALGORITHM);

        cipher.init(Cipher.DECRYPT_MODE, k);
 

        return cipher.doFinal(data);
    }

 
    /**

     * 加密
     *

     * @param data
     * @param key

     * @return
     * @throws Exception

     */
    public static byte[] encrypt(byte[] data, String key) throws Exception {

        Key k = toKey(decryptBASE64(key));
        Cipher cipher = Cipher.getInstance(ALGORITHM);

        cipher.init(Cipher.ENCRYPT_MODE, k);
 

        return cipher.doFinal(data);
    }

 
    /**

     * 生成密钥
     *

     * @return
     * @throws Exception

     */
    public static String initKey() throws Exception {

        return initKey(null);
    }

 
    /**

     * 生成密钥
     *

     * @param seed
     * @return

     * @throws Exception
     */

    public static String initKey(String seed) throws Exception {
        SecureRandom secureRandom = null;

 
        if (seed != null) {

            secureRandom = new SecureRandom(decryptBASE64(seed));
        } else {

            secureRandom = new SecureRandom();
        }

 
        KeyGenerator kg = KeyGenerator.getInstance(ALGORITHM);

        kg.init(secureRandom);
 

        SecretKey secretKey = kg.generateKey();
 

        return encryptBASE64(secretKey.getEncoded());
    }

}
延续上一个类的实现，我们通过MD5以及SHA对字符串加密生成密钥，这是比较常见的密钥生成方式。
再给出一个测试类：
Java代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. import static org.junit.Assert.*;  
1.   
1.   
1. import org.junit.Test;  
1.   
1. /** 
1.  *  
1.  * @author 梁栋 
1.  * @version 1.0 
1.  * @since 1.0 
1.  */  
1. public class DESCoderTest {  
1.   
1.     @Test  
1.     public void test() throws Exception {  
1.         String inputStr = "DES";  
1.         String key = DESCoder.initKey();  
1.         System.err.println("原文:\t" + inputStr);  
1.   
1.         System.err.println("密钥:\t" + key);  
1.   
1.         byte[] inputData = inputStr.getBytes();  
1.         inputData = DESCoder.encrypt(inputData, key);  
1.   
1.         System.err.println("加密后:\t" + DESCoder.encryptBASE64(inputData));  
1.   
1.         byte[] outputData = DESCoder.decrypt(inputData, key);  
1.         String outputStr = new String(outputData);  
1.   
1.         System.err.println("解密后:\t" + outputStr);  
1.   
1.         assertEquals(inputStr, outputStr);  
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

public class DESCoderTest {
 

    @Test
    public void test() throws Exception {

        String inputStr = "DES";
        String key = DESCoder.initKey();

        System.err.println("原文:\t" + inputStr);
 

        System.err.println("密钥:\t" + key);
 

        byte[] inputData = inputStr.getBytes();
        inputData = DESCoder.encrypt(inputData, key);

 
        System.err.println("加密后:\t" + DESCoder.encryptBASE64(inputData));

 
        byte[] outputData = DESCoder.decrypt(inputData, key);

        String outputStr = new String(outputData);
 

        System.err.println("解密后:\t" + outputStr);
 

        assertEquals(inputStr, outputStr);
    }

}
得到的输出内容如下：
Console代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. 原文: DES  
1. 密钥: f3wEtRrV6q0=  
1.   
1. 加密后:    C6qe9oNIzRY=  
1.   
1. 解密后:    DES  

原文:    DES

密钥:    f3wEtRrV6q0=
 

加密后:    C6qe9oNIzRY=
 

解密后:    DES
    由控制台得到的输出，我们能够比对加密、解密后结果一致。这是一种简单的加密解密方式，只有一个密钥。
    其实DES有很多同胞兄弟，如DESede(TripleDES)、AES、Blowfish、RC2、RC4(ARCFOUR)。这里就不过多阐述了，大同小异，只要换掉ALGORITHM换成对应的值，同时做一个代码替换**SecretKey secretKey = new SecretKeySpec(key, ALGORITHM);**就可以了，此外就是密钥长度不同了。
Java代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. /** 
1.  * DES          key size must be equal to 56 
1.  * DESede(TripleDES) key size must be equal to 112 or 168 
1.  * AES          key size must be equal to 128, 192 or 256,but 192 and 256 bits may not be available 
1.  * Blowfish     key size must be multiple of 8, and can only range from 32 to 448 (inclusive) 
1.  * RC2          key size must be between 40 and 1024 bits 
1.  * RC4(ARCFOUR) key size must be between 40 and 1024 bits 
1.  **/  

/**

 * DES          key size must be equal to 56
* DESede(TripleDES) key size must be equal to 112 or 168

 * AES          key size must be equal to 128, 192 or 256,but 192 and 256 bits may not be available
 * Blowfish     key size must be multiple of 8, and can only range from 32 to 448 (inclusive)

 * RC2          key size must be between 40 and 1024 bits
* RC4(ARCFOUR) key size must be between 40 and 1024 bits

**/
**相关链接：
[Java加密技术（一）——BASE64与单向加密算法MD5&SHA&MAC](http://snowolf.iteye.com/blog/379860)
[Java加密技术（二）——对称加密算法DES&AES](http://snowolf.iteye.com/blog/380034)
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
