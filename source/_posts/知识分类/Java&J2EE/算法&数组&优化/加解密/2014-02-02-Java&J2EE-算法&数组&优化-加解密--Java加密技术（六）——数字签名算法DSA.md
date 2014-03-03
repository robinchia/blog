---
layout: post
title: "Java加密技术（六）——数字签名算法DSA"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - 算法&数组&优化
 - 加解密
--- 

# Java加密技术（六）——数字签名算法DSA

    接下来我们介绍DSA数字签名，非对称加密的另一种实现。
**DSA**
DSA-Digital Signature Algorithm 是Schnorr和ElGamal签名算法的变种，被美国NIST作为DSS(DigitalSignature Standard)。简单的说，这是一种更高级的验证方式，用作数字签名。不单单只有公钥、私钥，还有数字签名。私钥加密生成数字签名，公钥验证数据及签名。如果数据和签名不匹配则认为验证失败！数字签名的作用就是校验数据在传输过程中不被修改。数字签名，是单向加密的升级！![]()

1. ![]()
1. ![]()
通过java代码实现如下：**Coder类见 [Java加密技术（一）](http://snowolf.iteye.com/blog/379860)**
Java代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. import java.security.Key;  
1. import java.security.KeyFactory;  
1. import java.security.KeyPair;  
1. import java.security.KeyPairGenerator;  
1. import java.security.PrivateKey;  
1. import java.security.PublicKey;  
1. import java.security.SecureRandom;  
1. import java.security.Signature;  
1. import java.security.interfaces.DSAPrivateKey;  
1. import java.security.interfaces.DSAPublicKey;  
1. import java.security.spec.PKCS8EncodedKeySpec;  
1. import java.security.spec.X509EncodedKeySpec;  
1. import java.util.HashMap;  
1. import java.util.Map;  
1.   
1. /** 
1.  * DSA安全编码组件 
1.  *  
1.  * @author 梁栋 
1.  * @version 1.0 
1.  * @since 1.0 
1.  */  
1. public abstract class DSACoder extends Coder {  
1.   
1.     public static final String ALGORITHM = "DSA";  
1.   
1.     /** 
1.      * 默认密钥字节数 
1.      *  
1.      * <pre> 
1.      * DSA  
1.      * Default Keysize 1024   
1.      * Keysize must be a multiple of 64, ranging from 512 to 1024 (inclusive). 
1.      * </pre> 
1.      */  
1.     private static final int KEY_SIZE = 1024;  
1.   
1.     /** 
1.      * 默认种子 
1.      */  
1.     private static final String DEFAULT_SEED = "0f22507a10bbddd07d8a3082122966e3";  
1.   
1.     private static final String PUBLIC_KEY = "DSAPublicKey";  
1.     private static final String PRIVATE_KEY = "DSAPrivateKey";  
1.   
1.     /** 
1.      * 用私钥对信息生成数字签名 
1.      *  
1.      * @param data 
1.      *            加密数据 
1.      * @param privateKey 
1.      *            私钥 
1.      *  
1.      * @return 
1.      * @throws Exception 
1.      */  
1.     public static String sign(byte[] data, String privateKey) throws Exception {  
1.         // 解密由base64编码的私钥  
1.         byte[] keyBytes = decryptBASE64(privateKey);  
1.   
1.         // 构造PKCS8EncodedKeySpec对象  
1.         PKCS8EncodedKeySpec pkcs8KeySpec = new PKCS8EncodedKeySpec(keyBytes);  
1.   
1.         // KEY_ALGORITHM 指定的加密算法  
1.         KeyFactory keyFactory = KeyFactory.getInstance(ALGORITHM);  
1.   
1.         // 取私钥匙对象  
1.         PrivateKey priKey = keyFactory.generatePrivate(pkcs8KeySpec);  
1.   
1.         // 用私钥对信息生成数字签名  
1.         Signature signature = Signature.getInstance(keyFactory.getAlgorithm());  
1.         signature.initSign(priKey);  
1.         signature.update(data);  
1.   
1.         return encryptBASE64(signature.sign());  
1.     }  
1.   
1.     /** 
1.      * 校验数字签名 
1.      *  
1.      * @param data 
1.      *            加密数据 
1.      * @param publicKey 
1.      *            公钥 
1.      * @param sign 
1.      *            数字签名 
1.      *  
1.      * @return 校验成功返回true 失败返回false 
1.      * @throws Exception 
1.      *  
1.      */  
1.     public static boolean verify(byte[] data, String publicKey, String sign)  
1.             throws Exception {  
1.   
1.         // 解密由base64编码的公钥  
1.         byte[] keyBytes = decryptBASE64(publicKey);  
1.   
1.         // 构造X509EncodedKeySpec对象  
1.         X509EncodedKeySpec keySpec = new X509EncodedKeySpec(keyBytes);  
1.   
1.         // ALGORITHM 指定的加密算法  
1.         KeyFactory keyFactory = KeyFactory.getInstance(ALGORITHM);  
1.   
1.         // 取公钥匙对象  
1.         PublicKey pubKey = keyFactory.generatePublic(keySpec);  
1.   
1.         Signature signature = Signature.getInstance(keyFactory.getAlgorithm());  
1.         signature.initVerify(pubKey);  
1.         signature.update(data);  
1.   
1.         // 验证签名是否正常  
1.         return signature.verify(decryptBASE64(sign));  
1.     }  
1.   
1.     /** 
1.      * 生成密钥 
1.      *  
1.      * @param seed 
1.      *            种子 
1.      * @return 密钥对象 
1.      * @throws Exception 
1.      */  
1.     public static Map<String, Object> initKey(String seed) throws Exception {  
1.         KeyPairGenerator keygen = KeyPairGenerator.getInstance(ALGORITHM);  
1.         // 初始化随机产生器  
1.         SecureRandom secureRandom = new SecureRandom();  
1.         secureRandom.setSeed(seed.getBytes());  
1.         keygen.initialize(KEY_SIZE, secureRandom);  
1.   
1.         KeyPair keys = keygen.genKeyPair();  
1.   
1.         DSAPublicKey publicKey = (DSAPublicKey) keys.getPublic();  
1.         DSAPrivateKey privateKey = (DSAPrivateKey) keys.getPrivate();  
1.   
1.         Map<String, Object> map = new HashMap<String, Object>(2);  
1.         map.put(PUBLIC_KEY, publicKey);  
1.         map.put(PRIVATE_KEY, privateKey);  
1.   
1.         return map;  
1.     }  
1.   
1.     /** 
1.      * 默认生成密钥 
1.      *  
1.      * @return 密钥对象 
1.      * @throws Exception 
1.      */  
1.     public static Map<String, Object> initKey() throws Exception {  
1.         return initKey(DEFAULT_SEED);  
1.     }  
1.   
1.     /** 
1.      * 取得私钥 
1.      *  
1.      * @param keyMap 
1.      * @return 
1.      * @throws Exception 
1.      */  
1.     public static String getPrivateKey(Map<String, Object> keyMap)  
1.             throws Exception {  
1.         Key key = (Key) keyMap.get(PRIVATE_KEY);  
1.   
1.         return encryptBASE64(key.getEncoded());  
1.     }  
1.   
1.     /** 
1.      * 取得公钥 
1.      *  
1.      * @param keyMap 
1.      * @return 
1.      * @throws Exception 
1.      */  
1.     public static String getPublicKey(Map<String, Object> keyMap)  
1.             throws Exception {  
1.         Key key = (Key) keyMap.get(PUBLIC_KEY);  
1.   
1.         return encryptBASE64(key.getEncoded());  
1.     }  
1. }  

import java.security.Key;

import java.security.KeyFactory;
import java.security.KeyPair;

import java.security.KeyPairGenerator;
import java.security.PrivateKey;

import java.security.PublicKey;
import java.security.SecureRandom;

import java.security.Signature;
import java.security.interfaces.DSAPrivateKey;

import java.security.interfaces.DSAPublicKey;
import java.security.spec.PKCS8EncodedKeySpec;

import java.security.spec.X509EncodedKeySpec;
import java.util.HashMap;

import java.util.Map;
 

/**
* DSA安全编码组件

*
* @author 梁栋

* @version 1.0
* @since 1.0

*/
public abstract class DSACoder extends Coder {

 
    public static final String ALGORITHM = "DSA";

 
    /**

     * 默认密钥字节数
     *

     * <pre>
     * DSA

     * Default Keysize 1024 
     * Keysize must be a multiple of 64, ranging from 512 to 1024 (inclusive).

     * </pre>
     */

    private static final int KEY_SIZE = 1024;
 

    /**
     * 默认种子

     */
    private static final String DEFAULT_SEED = "0f22507a10bbddd07d8a3082122966e3";

 
    private static final String PUBLIC_KEY = "DSAPublicKey";

    private static final String PRIVATE_KEY = "DSAPrivateKey";
 

    /**
     * 用私钥对信息生成数字签名

     *
     * @param data

     *            加密数据
     * @param privateKey

     *            私钥
     *

     * @return
     * @throws Exception

     */
    public static String sign(byte[] data, String privateKey) throws Exception {

        // 解密由base64编码的私钥
        byte[] keyBytes = decryptBASE64(privateKey);

 
        // 构造PKCS8EncodedKeySpec对象

        PKCS8EncodedKeySpec pkcs8KeySpec = new PKCS8EncodedKeySpec(keyBytes);
 

        // KEY_ALGORITHM 指定的加密算法
        KeyFactory keyFactory = KeyFactory.getInstance(ALGORITHM);

 
        // 取私钥匙对象

        PrivateKey priKey = keyFactory.generatePrivate(pkcs8KeySpec);
 

        // 用私钥对信息生成数字签名
        Signature signature = Signature.getInstance(keyFactory.getAlgorithm());

        signature.initSign(priKey);
        signature.update(data);

 
        return encryptBASE64(signature.sign());

    }
 

    /**
     * 校验数字签名

     *
     * @param data

     *            加密数据
     * @param publicKey

     *            公钥
     * @param sign

     *            数字签名
     *

     * @return 校验成功返回true 失败返回false
     * @throws Exception

     *
     */

    public static boolean verify(byte[] data, String publicKey, String sign)
            throws Exception {

 
        // 解密由base64编码的公钥

        byte[] keyBytes = decryptBASE64(publicKey);
 

        // 构造X509EncodedKeySpec对象
        X509EncodedKeySpec keySpec = new X509EncodedKeySpec(keyBytes);

 
        // ALGORITHM 指定的加密算法

        KeyFactory keyFactory = KeyFactory.getInstance(ALGORITHM);
 

        // 取公钥匙对象
        PublicKey pubKey = keyFactory.generatePublic(keySpec);

 
        Signature signature = Signature.getInstance(keyFactory.getAlgorithm());

        signature.initVerify(pubKey);
        signature.update(data);

 
        // 验证签名是否正常

        return signature.verify(decryptBASE64(sign));
    }

 
    /**

     * 生成密钥
     *

     * @param seed
     *            种子

     * @return 密钥对象
     * @throws Exception

     */
    public static Map<String, Object> initKey(String seed) throws Exception {

        KeyPairGenerator keygen = KeyPairGenerator.getInstance(ALGORITHM);
        // 初始化随机产生器

        SecureRandom secureRandom = new SecureRandom();
        secureRandom.setSeed(seed.getBytes());

        keygen.initialize(KEY_SIZE, secureRandom);
 

        KeyPair keys = keygen.genKeyPair();
 

        DSAPublicKey publicKey = (DSAPublicKey) keys.getPublic();
        DSAPrivateKey privateKey = (DSAPrivateKey) keys.getPrivate();

 
        Map<String, Object> map = new HashMap<String, Object>(2);

        map.put(PUBLIC_KEY, publicKey);
        map.put(PRIVATE_KEY, privateKey);

 
        return map;

    }
 

    /**
     * 默认生成密钥

     *
     * @return 密钥对象

     * @throws Exception
     */

    public static Map<String, Object> initKey() throws Exception {
        return initKey(DEFAULT_SEED);

    }
 

    /**
     * 取得私钥

     *
     * @param keyMap

     * @return
     * @throws Exception

     */
    public static String getPrivateKey(Map<String, Object> keyMap)

            throws Exception {
        Key key = (Key) keyMap.get(PRIVATE_KEY);

 
        return encryptBASE64(key.getEncoded());

    }
 

    /**
     * 取得公钥

     *
     * @param keyMap

     * @return
     * @throws Exception

     */
    public static String getPublicKey(Map<String, Object> keyMap)

            throws Exception {
        Key key = (Key) keyMap.get(PUBLIC_KEY);

 
        return encryptBASE64(key.getEncoded());

    }
}
再给出一个测试类：
Java代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. import static org.junit.Assert.*;  
1.   
1. import java.util.Map;  
1.   
1. import org.junit.Test;  
1.   
1. /** 
1.  *  
1.  * @author 梁栋 
1.  * @version 1.0 
1.  * @since 1.0 
1.  */  
1. public class DSACoderTest {  
1.   
1.     @Test  
1.     public void test() throws Exception {  
1.         String inputStr = "abc";  
1.         byte[] data = inputStr.getBytes();  
1.   
1.         // 构建密钥  
1.         Map<String, Object> keyMap = DSACoder.initKey();  
1.   
1.         // 获得密钥  
1.         String publicKey = DSACoder.getPublicKey(keyMap);  
1.         String privateKey = DSACoder.getPrivateKey(keyMap);  
1.   
1.         System.err.println("公钥:\r" + publicKey);  
1.         System.err.println("私钥:\r" + privateKey);  
1.   
1.         // 产生签名  
1.         String sign = DSACoder.sign(data, privateKey);  
1.         System.err.println("签名:\r" + sign);  
1.   
1.         // 验证签名  
1.         boolean status = DSACoder.verify(data, publicKey, sign);  
1.         System.err.println("状态:\r" + status);  
1.         assertTrue(status);  
1.   
1.     }  
1.   
1. }  

import static org.junit.Assert.*;

 
import java.util.Map;

 
import org.junit.Test;

 
/**

*
* @author 梁栋

* @version 1.0
* @since 1.0

*/
public class DSACoderTest {

 
    @Test

    public void test() throws Exception {
        String inputStr = "abc";

        byte[] data = inputStr.getBytes();
 

        // 构建密钥
        Map<String, Object> keyMap = DSACoder.initKey();

 
        // 获得密钥

        String publicKey = DSACoder.getPublicKey(keyMap);
        String privateKey = DSACoder.getPrivateKey(keyMap);

 
        System.err.println("公钥:\r" + publicKey);

        System.err.println("私钥:\r" + privateKey);
 

        // 产生签名
        String sign = DSACoder.sign(data, privateKey);

        System.err.println("签名:\r" + sign);
 

        // 验证签名
        boolean status = DSACoder.verify(data, publicKey, sign);

        System.err.println("状态:\r" + status);
        assertTrue(status);

 
    }

 
}
控制台输出：
Console代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. 公钥:  
1. MIIBtzCCASwGByqGSM44BAEwggEfAoGBAP1/U4EddRIpUt9KnC7s5Of2EbdSPO9EAMMeP4C2USZp  
1. RV1AIlH7WT2NWPq/xfW6MPbLm1Vs14E7gB00b/JmYLdrmVClpJ+f6AR7ECLCT7up1/63xhv4O1fn  
1. xqimFQ8E+4P208UewwI1VBNaFpEy9nXzrith1yrv8iIDGZ3RSAHHAhUAl2BQjxUjC8yykrmCouuE  
1. C/BYHPUCgYEA9+GghdabPd7LvKtcNrhXuXmUr7v6OuqC+VdMCz0HgmdRWVeOutRZT+ZxBxCBgLRJ  
1. FnEj6EwoFhO3zwkyjMim4TwWeotUfI0o4KOuHiuzpnWRbqN/C/ohNWLx+2J6ASQ7zKTxvqhRkImo  
1. g9/hWuWfBpKLZl6Ae1UlZAFMO/7PSSoDgYQAAoGAIu4RUlcQLp49PI0MrbssOY+3uySVnp0TULSv  
1. 5T4VaHoKzsLHgGTrwOvsGA+V3yCNl2WDu3D84bSLF7liTWgOj+SMOEaPk4VyRTlLXZWGPsf1Mfd9  
1. 21XAbMeVyKDSHHVGbMjBScajf3bXooYQMlyoHiOt/WrCo+mv7efstMM0PGo=  
1.   
1. 私钥:  
1. MIIBTAIBADCCASwGByqGSM44BAEwggEfAoGBAP1/U4EddRIpUt9KnC7s5Of2EbdSPO9EAMMeP4C2  
1. USZpRV1AIlH7WT2NWPq/xfW6MPbLm1Vs14E7gB00b/JmYLdrmVClpJ+f6AR7ECLCT7up1/63xhv4  
1. O1fnxqimFQ8E+4P208UewwI1VBNaFpEy9nXzrith1yrv8iIDGZ3RSAHHAhUAl2BQjxUjC8yykrmC  
1. ouuEC/BYHPUCgYEA9+GghdabPd7LvKtcNrhXuXmUr7v6OuqC+VdMCz0HgmdRWVeOutRZT+ZxBxCB  
1. gLRJFnEj6EwoFhO3zwkyjMim4TwWeotUfI0o4KOuHiuzpnWRbqN/C/ohNWLx+2J6ASQ7zKTxvqhR  
1. kImog9/hWuWfBpKLZl6Ae1UlZAFMO/7PSSoEFwIVAIegLUtmm2oQKQJTOiLugHTSjl/q  
1.   
1. 签名:  
1. MC0CFQCMg0J/uZmF8GuRpr3TNq48w60nDwIUJCyYNah+HtbU6NcQfy8Ac6LeLQs=  
1.   
1. 状态:  
1. true  

公钥:

MIIBtzCCASwGByqGSM44BAEwggEfAoGBAP1/U4EddRIpUt9KnC7s5Of2EbdSPO9EAMMeP4C2USZp
RV1AIlH7WT2NWPq/xfW6MPbLm1Vs14E7gB00b/JmYLdrmVClpJ+f6AR7ECLCT7up1/63xhv4O1fn

xqimFQ8E+4P208UewwI1VBNaFpEy9nXzrith1yrv8iIDGZ3RSAHHAhUAl2BQjxUjC8yykrmCouuE
C/BYHPUCgYEA9+GghdabPd7LvKtcNrhXuXmUr7v6OuqC+VdMCz0HgmdRWVeOutRZT+ZxBxCBgLRJ

FnEj6EwoFhO3zwkyjMim4TwWeotUfI0o4KOuHiuzpnWRbqN/C/ohNWLx+2J6ASQ7zKTxvqhRkImo
g9/hWuWfBpKLZl6Ae1UlZAFMO/7PSSoDgYQAAoGAIu4RUlcQLp49PI0MrbssOY+3uySVnp0TULSv

5T4VaHoKzsLHgGTrwOvsGA+V3yCNl2WDu3D84bSLF7liTWgOj+SMOEaPk4VyRTlLXZWGPsf1Mfd9
21XAbMeVyKDSHHVGbMjBScajf3bXooYQMlyoHiOt/WrCo+mv7efstMM0PGo=

 
私钥:

MIIBTAIBADCCASwGByqGSM44BAEwggEfAoGBAP1/U4EddRIpUt9KnC7s5Of2EbdSPO9EAMMeP4C2
USZpRV1AIlH7WT2NWPq/xfW6MPbLm1Vs14E7gB00b/JmYLdrmVClpJ+f6AR7ECLCT7up1/63xhv4

O1fnxqimFQ8E+4P208UewwI1VBNaFpEy9nXzrith1yrv8iIDGZ3RSAHHAhUAl2BQjxUjC8yykrmC
ouuEC/BYHPUCgYEA9+GghdabPd7LvKtcNrhXuXmUr7v6OuqC+VdMCz0HgmdRWVeOutRZT+ZxBxCB

gLRJFnEj6EwoFhO3zwkyjMim4TwWeotUfI0o4KOuHiuzpnWRbqN/C/ohNWLx+2J6ASQ7zKTxvqhR
kImog9/hWuWfBpKLZl6Ae1UlZAFMO/7PSSoEFwIVAIegLUtmm2oQKQJTOiLugHTSjl/q

 
签名:

MC0CFQCMg0J/uZmF8GuRpr3TNq48w60nDwIUJCyYNah+HtbU6NcQfy8Ac6LeLQs=
 

状态:
true
注意状态为true，就验证成功！![]()
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
