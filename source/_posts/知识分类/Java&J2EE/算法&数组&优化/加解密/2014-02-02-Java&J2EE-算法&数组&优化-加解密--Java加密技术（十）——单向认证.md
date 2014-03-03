---
layout: post
title: "Java加密技术（十）——单向认证"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - 算法&数组&优化
 - 加解密
--- 

# Java加密技术（十）——单向认证

    在**[Java 加密技术（九）](http://snowolf.iteye.com/blog/397693)**中，我们使用自签名证书完成了认证。接下来，我们使用第三方CA签名机构完成证书签名。
    这里我们使用[thawte](https://www.thawte.com/)提供的测试用21天免费ca证书。
    1.要在该网站上注明你的域名，这里使用**www.zlex.org**作为测试用域名（请勿使用该域名作为你的域名地址，该域名受法律保护！请使用其他非注册域名！）。
    2.如果域名有效，你会收到邮件要求你访问[https://www.thawte.com/cgi/server/try.exe](https://www.thawte.com/cgi/server/try.exe)获得ca证书。
    3.复述密钥库的创建。
   

Shell代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. keytool -genkey -validity 36000 -alias www.zlex.org -keyalg RSA -keystore d:\zlex.keystore  
keytool -genkey -validity 36000 -alias www.zlex.org -keyalg RSA -keystore d:\zlex.keystore
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
 
    4.通过如下命令，从zlex.keystore中导出CA证书申请。
   

Shell代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. keytool -certreq -alias www.zlex.org -file d:\zlex.csr -keystore d:\zlex.keystore -v  
keytool -certreq -alias www.zlex.org -file d:\zlex.csr -keystore d:\zlex.keystore -v你会获得zlex.csr文件，可以用记事本打开，内容如下格式：

Text代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. -----BEGIN NEW CERTIFICATE REQUEST-----  
1. MIIBnDCCAQUCAQAwXDELMAkGA1UEBhMCQ04xCzAJBgNVBAgTAkJKMQswCQYDVQQHEwJCSjENMAsG  
1. A1UEChMEemxleDENMAsGA1UECxMEemxleDEVMBMGA1UEAxMMd3d3LnpsZXgub3JnMIGfMA0GCSqG  
1. SIb3DQEBAQUAA4GNADCBiQKBgQCR6DXU9Mp+mCKO7cv9JPsj0n1Ec/GpM09qvhpgX3FNad/ZWSDc  
1. vU77YXZSoF9hQp3w1LC+eeKgd2MlVpXTvbVwBNVd2HiQPp37ic6BUUjSaX8LHtCl7l0BIEye9qQ2  
1. j8G0kak7e8ZA0s7nb3Ymq/K8BV7v0MQIdhIc1bifK9ZDewIDAQABoAAwDQYJKoZIhvcNAQEFBQAD  
1. gYEAMA1r2fbZPtNx37U9TRwadCH2TZZecwKJS/hskNm6ryPKIAp9APWwAyj8WJHRBz5SpZM4zmYO  
1. oMCI8BcnY2A4JP+R7/SwXTdH/xcg7NVghd9A2SCgqMpF7KMfc5dE3iygdiPu+UhY200Dvpjx8gmJ  
1. 1UbH3+nqMUyCrZgURFslOUY=  
1. -----END NEW CERTIFICATE REQUEST-----  
-----BEGIN NEW CERTIFICATE REQUEST-----

MIIBnDCCAQUCAQAwXDELMAkGA1UEBhMCQ04xCzAJBgNVBAgTAkJKMQswCQYDVQQHEwJCSjENMAsG
A1UEChMEemxleDENMAsGA1UECxMEemxleDEVMBMGA1UEAxMMd3d3LnpsZXgub3JnMIGfMA0GCSqG

SIb3DQEBAQUAA4GNADCBiQKBgQCR6DXU9Mp+mCKO7cv9JPsj0n1Ec/GpM09qvhpgX3FNad/ZWSDc
vU77YXZSoF9hQp3w1LC+eeKgd2MlVpXTvbVwBNVd2HiQPp37ic6BUUjSaX8LHtCl7l0BIEye9qQ2

j8G0kak7e8ZA0s7nb3Ymq/K8BV7v0MQIdhIc1bifK9ZDewIDAQABoAAwDQYJKoZIhvcNAQEFBQAD
gYEAMA1r2fbZPtNx37U9TRwadCH2TZZecwKJS/hskNm6ryPKIAp9APWwAyj8WJHRBz5SpZM4zmYO

oMCI8BcnY2A4JP+R7/SwXTdH/xcg7NVghd9A2SCgqMpF7KMfc5dE3iygdiPu+UhY200Dvpjx8gmJ
1UbH3+nqMUyCrZgURFslOUY=

-----END NEW CERTIFICATE REQUEST-----
    5.将上述文件内容拷贝到[https://www.thawte.com/cgi/server/try.exe](https://www.thawte.com/cgi/server/try.exe)中，点击next，获得回应内容，这里是p7b格式。
内容如下：

Text代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. -----BEGIN PKCS7-----  
1. MIIF3AYJKoZIhvcNAQcCoIIFzTCCBckCAQExADALBgkqhkiG9w0BBwGgggWxMIID  
1. EDCCAnmgAwIBAgIQA/mx/pKoaB+KGX2hveFU9zANBgkqhkiG9w0BAQUFADCBhzEL  
1. MAkGA1UEBhMCWkExIjAgBgNVBAgTGUZPUiBURVNUSU5HIFBVUlBPU0VTIE9OTFkx  
1. HTAbBgNVBAoTFFRoYXd0ZSBDZXJ0aWZpY2F0aW9uMRcwFQYDVQQLEw5URVNUIFRF  
1. U1QgVEVTVDEcMBoGA1UEAxMTVGhhd3RlIFRlc3QgQ0EgUm9vdDAeFw0wOTA1Mjgw  
1. MDIxMzlaFw0wOTA2MTgwMDIxMzlaMFwxCzAJBgNVBAYTAkNOMQswCQYDVQQIEwJC  
1. SjELMAkGA1UEBxMCQkoxDTALBgNVBAoTBHpsZXgxDTALBgNVBAsTBHpsZXgxFTAT  
1. BgNVBAMTDHd3dy56bGV4Lm9yZzCBnzANBgkqhkiG9w0BAQEFAAOBjQAwgYkCgYEA  
1. keg11PTKfpgiju3L/ST7I9J9RHPxqTNPar4aYF9xTWnf2Vkg3L1O+2F2UqBfYUKd  
1. 8NSwvnnioHdjJVaV0721cATVXdh4kD6d+4nOgVFI0ml/Cx7Qpe5dASBMnvakNo/B  
1. tJGpO3vGQNLO5292JqvyvAVe79DECHYSHNW4nyvWQ3sCAwEAAaOBpjCBozAMBgNV  
1. HRMBAf8EAjAAMB0GA1UdJQQWMBQGCCsGAQUFBwMBBggrBgEFBQcDAjBABgNVHR8E  
1. OTA3MDWgM6Axhi9odHRwOi8vY3JsLnRoYXd0ZS5jb20vVGhhd3RlUHJlbWl1bVNl  
1. cnZlckNBLmNybDAyBggrBgEFBQcBAQQmMCQwIgYIKwYBBQUHMAGGFmh0dHA6Ly9v  
1. Y3NwLnRoYXd0ZS5jb20wDQYJKoZIhvcNAQEFBQADgYEATPuxZbtJJSPmXvfrr1yz  
1. xqM06IwTZ6UU0lZRG7I0WufMjNMKdpn8hklUhE17mxAhGSpewLVVeLR7uzBLFkuC  
1. X7wMXxhoYdJZtNai72izU6Rd1oknao7diahvRxPK4IuQ7y2oZ511/4T4vgY6iRAj  
1. q4q76HhPJrVRL/sduaiu+gYwggKZMIICAqADAgECAgEAMA0GCSqGSIb3DQEBBAUA  
1. MIGHMQswCQYDVQQGEwJaQTEiMCAGA1UECBMZRk9SIFRFU1RJTkcgUFVSUE9TRVMg  
1. T05MWTEdMBsGA1UEChMUVGhhd3RlIENlcnRpZmljYXRpb24xFzAVBgNVBAsTDlRF  
1. U1QgVEVTVCBURVNUMRwwGgYDVQQDExNUaGF3dGUgVGVzdCBDQSBSb290MB4XDTk2  
1. MDgwMTAwMDAwMFoXDTIwMTIzMTIxNTk1OVowgYcxCzAJBgNVBAYTAlpBMSIwIAYD  
1. VQQIExlGT1IgVEVTVElORyBQVVJQT1NFUyBPTkxZMR0wGwYDVQQKExRUaGF3dGUg  
1. Q2VydGlmaWNhdGlvbjEXMBUGA1UECxMOVEVTVCBURVNUIFRFU1QxHDAaBgNVBAMT  
1. E1RoYXd0ZSBUZXN0IENBIFJvb3QwgZ8wDQYJKoZIhvcNAQEBBQADgY0AMIGJAoGB  
1. ALV9kG+Os6x/DOhm+tKUQfzVMWGhE95sFmEtkMMTX2Zi4n6i6BvzoReJ5njzt1LF  
1. cqu4EUk9Ji20egKKfmqRzmQFLP7+1niSdfJEUE7cKY40QoI99270PTrLjJeaMcCl  
1. +AYl+kD+RL5BtuKKU3PurYcsCsre6aTvjMcqpTJOGeSPAgMBAAGjEzARMA8GA1Ud  
1. EwEB/wQFMAMBAf8wDQYJKoZIhvcNAQEEBQADgYEAgozj7BkD9O8si2V0v+EZ/t7E  
1. fz/LC8y6mD7IBUziHy5/53ymGAGLtyhXHvX+UIE6UWbHro3IqVkrmY5uC93Z2Wew  
1. A/6edK3KFUcUikrLeewM7gmqsiASEKx2mKRKlu12jXyNS5tXrPWRDvUKtFC1uL9a  
1. 12rFAQS2BkIk7aU+ghYxAA==  
1. -----END PKCS7-----  
-----BEGIN PKCS7-----

MIIF3AYJKoZIhvcNAQcCoIIFzTCCBckCAQExADALBgkqhkiG9w0BBwGgggWxMIID
EDCCAnmgAwIBAgIQA/mx/pKoaB+KGX2hveFU9zANBgkqhkiG9w0BAQUFADCBhzEL

MAkGA1UEBhMCWkExIjAgBgNVBAgTGUZPUiBURVNUSU5HIFBVUlBPU0VTIE9OTFkx
HTAbBgNVBAoTFFRoYXd0ZSBDZXJ0aWZpY2F0aW9uMRcwFQYDVQQLEw5URVNUIFRF

U1QgVEVTVDEcMBoGA1UEAxMTVGhhd3RlIFRlc3QgQ0EgUm9vdDAeFw0wOTA1Mjgw
MDIxMzlaFw0wOTA2MTgwMDIxMzlaMFwxCzAJBgNVBAYTAkNOMQswCQYDVQQIEwJC

SjELMAkGA1UEBxMCQkoxDTALBgNVBAoTBHpsZXgxDTALBgNVBAsTBHpsZXgxFTAT
BgNVBAMTDHd3dy56bGV4Lm9yZzCBnzANBgkqhkiG9w0BAQEFAAOBjQAwgYkCgYEA

keg11PTKfpgiju3L/ST7I9J9RHPxqTNPar4aYF9xTWnf2Vkg3L1O+2F2UqBfYUKd
8NSwvnnioHdjJVaV0721cATVXdh4kD6d+4nOgVFI0ml/Cx7Qpe5dASBMnvakNo/B

tJGpO3vGQNLO5292JqvyvAVe79DECHYSHNW4nyvWQ3sCAwEAAaOBpjCBozAMBgNV
HRMBAf8EAjAAMB0GA1UdJQQWMBQGCCsGAQUFBwMBBggrBgEFBQcDAjBABgNVHR8E

OTA3MDWgM6Axhi9odHRwOi8vY3JsLnRoYXd0ZS5jb20vVGhhd3RlUHJlbWl1bVNl
cnZlckNBLmNybDAyBggrBgEFBQcBAQQmMCQwIgYIKwYBBQUHMAGGFmh0dHA6Ly9v

Y3NwLnRoYXd0ZS5jb20wDQYJKoZIhvcNAQEFBQADgYEATPuxZbtJJSPmXvfrr1yz
xqM06IwTZ6UU0lZRG7I0WufMjNMKdpn8hklUhE17mxAhGSpewLVVeLR7uzBLFkuC

X7wMXxhoYdJZtNai72izU6Rd1oknao7diahvRxPK4IuQ7y2oZ511/4T4vgY6iRAj
q4q76HhPJrVRL/sduaiu+gYwggKZMIICAqADAgECAgEAMA0GCSqGSIb3DQEBBAUA

MIGHMQswCQYDVQQGEwJaQTEiMCAGA1UECBMZRk9SIFRFU1RJTkcgUFVSUE9TRVMg
T05MWTEdMBsGA1UEChMUVGhhd3RlIENlcnRpZmljYXRpb24xFzAVBgNVBAsTDlRF

U1QgVEVTVCBURVNUMRwwGgYDVQQDExNUaGF3dGUgVGVzdCBDQSBSb290MB4XDTk2
MDgwMTAwMDAwMFoXDTIwMTIzMTIxNTk1OVowgYcxCzAJBgNVBAYTAlpBMSIwIAYD

VQQIExlGT1IgVEVTVElORyBQVVJQT1NFUyBPTkxZMR0wGwYDVQQKExRUaGF3dGUg
Q2VydGlmaWNhdGlvbjEXMBUGA1UECxMOVEVTVCBURVNUIFRFU1QxHDAaBgNVBAMT

E1RoYXd0ZSBUZXN0IENBIFJvb3QwgZ8wDQYJKoZIhvcNAQEBBQADgY0AMIGJAoGB
ALV9kG+Os6x/DOhm+tKUQfzVMWGhE95sFmEtkMMTX2Zi4n6i6BvzoReJ5njzt1LF

cqu4EUk9Ji20egKKfmqRzmQFLP7+1niSdfJEUE7cKY40QoI99270PTrLjJeaMcCl
+AYl+kD+RL5BtuKKU3PurYcsCsre6aTvjMcqpTJOGeSPAgMBAAGjEzARMA8GA1Ud

EwEB/wQFMAMBAf8wDQYJKoZIhvcNAQEEBQADgYEAgozj7BkD9O8si2V0v+EZ/t7E
fz/LC8y6mD7IBUziHy5/53ymGAGLtyhXHvX+UIE6UWbHro3IqVkrmY5uC93Z2Wew

A/6edK3KFUcUikrLeewM7gmqsiASEKx2mKRKlu12jXyNS5tXrPWRDvUKtFC1uL9a
12rFAQS2BkIk7aU+ghYxAA==

-----END PKCS7-----
将其存储为zlex.p7b
    6.将由CA签发的证书导入密钥库。
   

Shell代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. keytool -import -trustcacerts -alias www.zlex.org -file d:\zlex.p7b -keystore d:\zlex.keystore -v  
keytool -import -trustcacerts -alias www.zlex.org -file d:\zlex.p7b -keystore d:\zlex.keystore -v
在这里我使用的密码为 **123456**
    控制台输出：
Console代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. 输入keystore密码：  
1.   
1. 回复中的最高级认证：  
1.   
1. 所有者:CN=Thawte Test CA Root, OU=TEST TEST TEST, O=Thawte Certification, ST=FOR  
1.  TESTING PURPOSES ONLY, C=ZA  
1. 签发人:CN=Thawte Test CA Root, OU=TEST TEST TEST, O=Thawte Certification, ST=FOR  
1.  TESTING PURPOSES ONLY, C=ZA  
1. 序列号:0  
1. 有效期: Thu Aug 01 08:00:00 CST 1996 至Fri Jan 01 05:59:59 CST 2021  
1. 证书指纹:  
1.          MD5:5E:E0:0E:1D:17:B7:CA:A5:7D:36:D6:02:DF:4D:26:A4  
1.          SHA1:39:C6:9D:27:AF:DC:EB:47:D6:33:36:6A:B2:05:F1:47:A9:B4:DA:EA  
1.          签名算法名称:MD5withRSA  
1.          版本: 3  
1.   
1. 扩展:  
1.   
1. #1: ObjectId: 2.5.29.19 Criticality=true  
1. BasicConstraints:[  
1.   CA:true  
1.   PathLen:2147483647  
1. ]  
1.   
1.   
1. ... 是不可信的。 还是要安装回复？ [否]：  Y  
1. 认证回复已安装在 keystore中  
1. [正在存储 d:\zlex.keystore]  

输入keystore密码：

 
回复中的最高级认证：

 
所有者:CN=Thawte Test CA Root, OU=TEST TEST TEST, O=Thawte Certification, ST=FOR

TESTING PURPOSES ONLY, C=ZA
签发人:CN=Thawte Test CA Root, OU=TEST TEST TEST, O=Thawte Certification, ST=FOR

TESTING PURPOSES ONLY, C=ZA
序列号:0

有效期: Thu Aug 01 08:00:00 CST 1996 至Fri Jan 01 05:59:59 CST 2021
证书指纹:

         MD5:5E:E0:0E:1D:17:B7:CA:A5:7D:36:D6:02:DF:4D:26:A4
         SHA1:39:C6:9D:27:AF:DC:EB:47:D6:33:36:6A:B2:05:F1:47:A9:B4:DA:EA

         签名算法名称:MD5withRSA
         版本: 3

 
扩展:

 
#1: ObjectId: 2.5.29.19 Criticality=true

BasicConstraints:[
  CA:true

  PathLen:2147483647
]

 
 

... 是不可信的。 还是要安装回复？ [否]：  Y
认证回复已安装在 keystore中

[正在存储 d:\zlex.keystore]
    7.域名定位
    将域名www.zlex.org定位到本机上。打开C:\Windows\System32\drivers\etc\hosts文件，将www.zlex.org绑定在本机上。在文件末尾追加127.0.0.1       www.zlex.org。现在通过地址栏访问http://www.zlex.org，或者通过ping命令，如果能够定位到本机，域名映射就搞定了。
    8.配置server.xml
Xml代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. <Connector  
1.     keystoreFile="conf/zlex.keystore"  
1.     keystorePass="123456"   
1.     truststoreFile="conf/zlex.keystore"      
1.     truststorePass="123456"       
1.     SSLEnabled="true"  
1.     URIEncoding="UTF-8"  
1.     clientAuth="false"            
1.     maxThreads="150"  
1.     port="443"  
1.     protocol="HTTP/1.1"  
1.     scheme="https"  
1.     secure="true"  
1.     sslProtocol="TLS" />  

        <Connector

            keystoreFile="conf/zlex.keystore"
            keystorePass="123456"

            truststoreFile="conf/zlex.keystore"   
            truststorePass="123456"    

            SSLEnabled="true"
            URIEncoding="UTF-8"

            clientAuth="false"           
            maxThreads="150"

            port="443"
            protocol="HTTP/1.1"

            scheme="https"
            secure="true"

            sslProtocol="TLS" />
将文件**zlex.keystore**拷贝到tomcat的**conf**目录下，重新启动tomcat。访问[https://www.zlex.org/](https://www.zlex.org/)，我们发现联网有些迟钝。大约5秒钟后，网页正常显示，同时有如下图所示：
![]()
浏览器验证了该CA机构的有效性。
打开证书，如下图所示：
![]()
调整测试类：
Java代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. import static org.junit.Assert.*;  
1.   
1. import java.io.DataInputStream;  
1. import java.io.InputStream;  
1. import java.net.URL;  
1.   
1. import javax.net.ssl.HttpsURLConnection;  
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
1.   
1.     @Test  
1.     public void testHttps() throws Exception {  
1.         URL url = new URL("https://www.zlex.org/examples/");  
1.         HttpsURLConnection conn = (HttpsURLConnection) url.openConnection();  
1.   
1.         conn.setDoInput(true);  
1.         conn.setDoOutput(true);  
1.   
1.         CertificateCoder.configSSLSocketFactory(conn, password, keyStorePath,  
1.                 keyStorePath);  
1.   
1.         InputStream is = conn.getInputStream();  
1.   
1.         int length = conn.getContentLength();  
1.   
1.         DataInputStream dis = new DataInputStream(is);  
1.         byte[] data = new byte[length];  
1.         dis.readFully(data);  
1.   
1.         dis.close();  
1.         conn.disconnect();  
1.         System.err.println(new String(data));  
1.     }  
1. }  

import static org.junit.Assert.*;

 
import java.io.DataInputStream;

import java.io.InputStream;
import java.net.URL;

 
import javax.net.ssl.HttpsURLConnection;

 
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

 
    @Test

    public void testHttps() throws Exception {
        URL url = new URL("https://www.zlex.org/examples/");

        HttpsURLConnection conn = (HttpsURLConnection) url.openConnection();
 

        conn.setDoInput(true);
        conn.setDoOutput(true);

 
        CertificateCoder.configSSLSocketFactory(conn, password, keyStorePath,

                keyStorePath);
 

        InputStream is = conn.getInputStream();
 

        int length = conn.getContentLength();
 

        DataInputStream dis = new DataInputStream(is);
        byte[] data = new byte[length];

        dis.readFully(data);
 

        dis.close();
        conn.disconnect();

        System.err.println(new String(data));
    }

}
再次执行，验证通过！![]()
由此，我们了基于SSL协议的认证过程。测试类的testHttps方法模拟了一次浏览器的HTTPS访问。![]()
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
