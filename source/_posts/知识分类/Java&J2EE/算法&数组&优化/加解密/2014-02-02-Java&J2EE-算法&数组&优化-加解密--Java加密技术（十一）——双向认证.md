---
layout: post
title: "Java加密技术（十一）——双向认证"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - 算法&数组&优化
 - 加解密
--- 

# Java加密技术（十一）——双向认证

对于双向认证，做一个简单的描述。
服务器端下发证书，客户端接受证书。证书带有公钥信息，用于验证服务器端、对数据加密/解密，起到OSI五类服务的认证（鉴别）服务和保密性服务。
这只是单向认证，为什么？因为客户端可以验证服务器端，但服务器端不能验证客户端！
如果客户端也有这样一个证书，服务器端也就能够验证客户端，这就是双向认证了！
换言之，当你用银行的“U盾”之类的U盘与银行账户交互时，在你验证银行服务器的同时，服务器也在验证你！这种双重验证，正是网银系统的安全关键！
单向认证见**[Java加密技术（十)](http://snowolf.iteye.com/blog/398198)**
双向认证需要一个CA机构签发这样的客户端、服务器端证书，首先需要CA机构构建一个根证书。keytool可以构建证书但不能构建我们需要的根证书，openssl则可以！
根证书签发客户端证书，根私钥签发服务器端证书！
我们直接使用linux下的openssl来完成CA，需要修改openssl.cnf文件，在ubuntu下的**/etc/ssl/**目录下，找到[ CA_default ]修改dir变量。
原文
引用

[ CA_default ]
#dir = ./demoCA # Where everything is kept
我们把c盘的ca目录作为CA认证的根目录，文件修改后如下所示：
引用

[ CA_default ]
dir = $ENV::HOME/ca # Where everything is kept
我们需要在用户目录下构建一个ca目录，以及子目录，如下所下：
ca
|__certs
|__newcerts
|__private
|__crl
执行如下操作：
Shell代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. #!/bin/bash     
1.   
1. ca_path=ca  
1. certs_path=$ca_path/certs  
1. newcerts_path=$ca_path/newcerts  
1. private_path=$ca_path/private  
1. crl_path=$ca_path/crl  
1.   
1. echo 移除CA根目录     
1. rm -rf ca     
1.     
1. echo 构建CA根目录     
1. mkdir ca     
1.    
1. echo 构建子目录     
1. mkdir certs     
1. mkdir newcerts     
1. mkdir private     
1. mkdir crl    
1.   
1. #构建文件     
1. touch $ca_path/index.txt  
1. echo 01 > $ca_path/serial  
1. echo      
1.   
1. #构建随机数     
1. openssl rand -out $private_path/.rand 1000  
1. echo      
1.   
1. echo 生成根证书私钥     
1. openssl genrsa -des3 -out $private_path/ca.pem 2048  
1. echo     
1.   
1. echo 查看私钥信息  
1. openssl rsa -noout -text -in $private_path/ca.pem  
1. echo  
1.   
1. echo 生成根证书请求      
1. openssl req -new -key $private_path/ca.pem -out $certs_path/ca.csr -subj "/C=CN/ST=BJ/L=BJ/O=zlex/OU=zlex/CN=ca.zlex.org"  
1. echo     
1.   
1. echo 查看证书请求   
1. openssl req -in $certs_path/ca.csr -text -noout  
1. echo   
1.   
1. echo 签发根证书     
1. openssl ca -create_serial -out $certs_path/ca.crt -days 3650 -batch -keyfile $private_path/ca.pem -selfsign -extensions v3_ca -infiles $certs_path/ca.csr   
1. #openssl x509 -req -sha1 -extensions v3_ca -signkey $private_path/ca.pem -in $certs_path/ca.csr -out $certs_path/ca.crt -days 3650  
1. echo     
1.   
1. echo 查看证书详情  
1. openssl x509 -in $certs_path/ca.crt -text -noout  
1. echo  
1.   
1. echo 证书转换——根证书     
1. openssl pkcs12 -export -clcerts -in $certs_path/ca.crt -inkey $private_path/ca.pem -out $certs_path/ca.p12  
1. echo    
1.   
1. echo 生成服务器端私钥     
1. openssl genrsa -des3 -out $private_path/server.pem 1024  
1. echo     
1.   
1. echo 查看私钥信息  
1. openssl rsa -noout -text -in $private_path/server.pem  
1. echo  
1.   
1. echo 生成服务器端证书请求     
1. openssl req -new -key $private_path/server.pem -out $certs_path/server.csr -subj "/C=CN/ST=BJ/L=BJ/O=zlex/OU=zlex/CN=www.zlex.org"  
1. echo     
1.   
1. echo 查看证书请求   
1. openssl req -in $certs_path/server.csr -text -noout  
1. echo   
1.   
1. echo 签发服务器端证书  
1. openssl ca -in $certs_path/server.csr -out $certs_path/server.crt -cert $certs_path/ca.crt -keyfile $private_path/ca.pem -days 365 -notext  
1. #openssl x509 -req -days 365 -sha1 -extensions v3_req -CA $certs_path/ca.crt -CAkey $private_path/ca.pem -CAserial $ca_path/serial -CAcreateserial -in $certs_path/server.csr -out $certs_path/server.crt  
1. echo  
1.   
1. echo 查看证书详情  
1. openssl x509 -in $certs_path/server.crt -text -noout  
1. echo  
1.   
1. echo 证书转换——服务器端     
1. openssl pkcs12 -export -clcerts -in $certs_path/server.crt -inkey $private_path/server.pem -out $certs_path/server.p12  
1. echo      
1.   
1. echo 生成客户端私钥     
1. openssl genrsa -des3 -out $private_path/client.pem 1024  
1. echo     
1.   
1. echo 生成客户端私钥     
1. openssl genrsa -des3 -out $private_path/client.pem 1024  
1. echo     
1.   
1. echo 查看私钥信息  
1. openssl rsa -noout -text -in $private_path/client.pem  
1. echo  
1.   
1. echo 生成客户端证书请求     
1. openssl req -new -key $private_path/client.pem -out $certs_path/client.csr -subj "/C=CN/ST=BJ/L=BJ/O=zlex/OU=zlex/CN=zlex"  
1. echo     
1.   
1. echo 查看证书请求   
1. openssl req -in $certs_path/client.csr -text -noout  
1. echo   
1.   
1. echo 签发客户端证书     
1. openssl ca -in $certs_path/client.csr -out $certs_path/client.crt -cert $certs_path/ca.crt -keyfile $private_path/ca.pem -days 365 -notext  
1. #openssl x509 -req -days 365 -sha1 -extensions dir_sect -CA $certs_path/ca.crt -CAkey $private_path/ca.pem -CAserial $ca_path/serial -in $certs_path/client.csr -out $certs_path/client.crt  
1. echo     
1.   
1. echo 查看证书详情  
1. openssl x509 -in $certs_path/client.crt -text -noout  
1. echo  
1.   
1. echo 证书转换——客户端     
1. openssl pkcs12 -export -clcerts -in $certs_path/client.crt -inkey $private_path/client.pem -out $certs_path/client.p12  
1. echo   
1.   
1. echo 生成证书链PKCS#7  
1. openssl crl2pkcs7 -nocrl -certfile $certs_path/server.crt -certfile $certs_path/ca.crt -certfile $certs_path/client.crt -out  
1. form PEM -out $certs_path/zlex.p7b  
1. echo  
1.   
1. echo 查看证书链  
1. openssl pkcs7 -in $certs_path/zlex.p7b -print_certs -noout  

#!/bin/bash  

 
ca_path=ca

certs_path=$ca_path/certs
newcerts_path=$ca_path/newcerts

private_path=$ca_path/private
crl_path=$ca_path/crl

 
echo 移除CA根目录  

rm -rf ca  
 

echo 构建CA根目录  
mkdir ca  

 
echo 构建子目录  

mkdir certs  
mkdir newcerts  

mkdir private  
mkdir crl 

 
#构建文件  

touch $ca_path/index.txt
echo 01 > $ca_path/serial

echo   
 

#构建随机数  
openssl rand -out $private_path/.rand 1000

echo   
 

echo 生成根证书私钥  
openssl genrsa -des3 -out $private_path/ca.pem 2048

echo  
 

echo 查看私钥信息
openssl rsa -noout -text -in $private_path/ca.pem

echo
 

echo 生成根证书请求   
openssl req -new -key $private_path/ca.pem -out $certs_path/ca.csr -subj "/C=CN/ST=BJ/L=BJ/O=zlex/OU=zlex/CN=ca.zlex.org"

echo  
 

echo 查看证书请求
openssl req -in $certs_path/ca.csr -text -noout

echo
 

echo 签发根证书  
openssl ca -create_serial -out $certs_path/ca.crt -days 3650 -batch -keyfile $private_path/ca.pem -selfsign -extensions v3_ca -infiles $certs_path/ca.csr

#openssl x509 -req -sha1 -extensions v3_ca -signkey $private_path/ca.pem -in $certs_path/ca.csr -out $certs_path/ca.crt -days 3650
echo  

 
echo 查看证书详情

openssl x509 -in $certs_path/ca.crt -text -noout
echo

 
echo 证书转换——根证书  

openssl pkcs12 -export -clcerts -in $certs_path/ca.crt -inkey $private_path/ca.pem -out $certs_path/ca.p12
echo 

 
echo 生成服务器端私钥  

openssl genrsa -des3 -out $private_path/server.pem 1024
echo  

 
echo 查看私钥信息

openssl rsa -noout -text -in $private_path/server.pem
echo

 
echo 生成服务器端证书请求  

openssl req -new -key $private_path/server.pem -out $certs_path/server.csr -subj "/C=CN/ST=BJ/L=BJ/O=zlex/OU=zlex/CN=www.zlex.org"
echo  

 
echo 查看证书请求

openssl req -in $certs_path/server.csr -text -noout
echo

 
echo 签发服务器端证书

openssl ca -in $certs_path/server.csr -out $certs_path/server.crt -cert $certs_path/ca.crt -keyfile $private_path/ca.pem -days 365 -notext
#openssl x509 -req -days 365 -sha1 -extensions v3_req -CA $certs_path/ca.crt -CAkey $private_path/ca.pem -CAserial $ca_path/serial -CAcreateserial -in $certs_path/server.csr -out $certs_path/server.crt

echo
 

echo 查看证书详情
openssl x509 -in $certs_path/server.crt -text -noout

echo
 

echo 证书转换——服务器端  
openssl pkcs12 -export -clcerts -in $certs_path/server.crt -inkey $private_path/server.pem -out $certs_path/server.p12

echo   
 

echo 生成客户端私钥  
openssl genrsa -des3 -out $private_path/client.pem 1024

echo  
 

echo 生成客户端私钥  
openssl genrsa -des3 -out $private_path/client.pem 1024

echo  
 

echo 查看私钥信息
openssl rsa -noout -text -in $private_path/client.pem

echo
 

echo 生成客户端证书请求  
openssl req -new -key $private_path/client.pem -out $certs_path/client.csr -subj "/C=CN/ST=BJ/L=BJ/O=zlex/OU=zlex/CN=zlex"

echo  
 

echo 查看证书请求
openssl req -in $certs_path/client.csr -text -noout

echo
 

echo 签发客户端证书  
openssl ca -in $certs_path/client.csr -out $certs_path/client.crt -cert $certs_path/ca.crt -keyfile $private_path/ca.pem -days 365 -notext

#openssl x509 -req -days 365 -sha1 -extensions dir_sect -CA $certs_path/ca.crt -CAkey $private_path/ca.pem -CAserial $ca_path/serial -in $certs_path/client.csr -out $certs_path/client.crt
echo  

 
echo 查看证书详情

openssl x509 -in $certs_path/client.crt -text -noout
echo

 
echo 证书转换——客户端  

openssl pkcs12 -export -clcerts -in $certs_path/client.crt -inkey $private_path/client.pem -out $certs_path/client.p12
echo

 
echo 生成证书链PKCS#7

openssl crl2pkcs7 -nocrl -certfile $certs_path/server.crt -certfile $certs_path/ca.crt -certfile $certs_path/client.crt -out
form PEM -out $certs_path/zlex.p7b

echo
 

echo 查看证书链
openssl pkcs7 -in $certs_path/zlex.p7b -print_certs -noout
这个脚本就是最重要的结晶了！![]()
执行结果，如下：
引用

生成根证书私钥
Generating RSA private key, 2048 bit long modulus
..................................+++
.............................................................+++
e is 65537 (0x10001)
Enter pass phrase for ca/private/ca.pem:
Verifying - Enter pass phrase for ca/private/ca.pem:
查看私钥信息
Enter pass phrase for ca/private/ca.pem:
Private-Key: (2048 bit)
modulus:
    00:d4:18:ab:5f:ad:b7:d0:09:d4:68:63:b5:db:8a:
    d1:a1:db:7e:f3:bb:bb:c2:be:a7:35:17:9e:bb:20:
    d3:1f:ed:63:e7:7d:29:6d:d2:7c:60:06:47:53:a6:
    23:b0:bd:94:65:3f:57:1e:00:51:f3:a1:9a:1b:83:
    14:a5:53:72:86:21:a2:57:22:2f:6a:a9:46:50:8c:
    f0:51:cf:e6:83:5b:23:dc:f9:ea:6c:2e:51:20:61:
    d1:84:9f:28:e8:01:89:b5:cb:55:68:4a:11:b1:06:
    56:31:21:16:c8:ac:2b:68:31:e1:de:12:d3:21:12:
    83:36:4c:ca:a8:b5:7e:b9:a7:63:4e:8e:e0:79:0f:
    0e:91:36:28:7c:dd:9a:e2:e0:98:8b:91:7f:09:7d:
    20:bb:37:f2:ab:aa:f0:ef:ae:68:7e:db:ca:db:33:
    84:48:5a:e3:ff:0b:08:0e:96:6d:01:c8:12:35:ec:
    9f:31:55:7f:53:7e:bd:fb:c4:16:b8:1f:17:29:42:
    0f:0e:04:57:14:18:fd:e5:d6:3f:40:04:cd:85:dd:
    d3:eb:2f:9a:bf:3c:8a:60:01:88:2f:43:0a:8b:bb:
    50:13:f8:cc:68:f9:10:eb:f9:7e:63:de:62:55:32:
    a8:fe:ce:51:67:79:c9:a6:3b:a3:c9:d7:81:7c:48:
    f3:d1
publicExponent: 65537 (0x10001)
privateExponent:
    00:b0:8a:e4:43:1c:df:6e:bc:6f:e0:80:76:c4:8a:
    75:5a:0b:d1:4d:61:cb:b5:1b:6b:24:c7:47:69:ad:
    b5:ee:d2:73:a1:21:4e:95:ca:69:9a:a8:3f:40:c2:
    7e:dc:c3:c0:bc:d2:0f:5a:ba:9b:7c:76:dc:46:e0:
    42:14:27:34:a1:af:67:68:ad:dc:d8:24:94:91:c1:
    ee:db:ba:78:be:87:e3:7f:31:4b:4e:c6:f2:e2:48:
    69:d4:c1:82:94:33:8b:84:15:ff:3e:72:c0:ed:20:
    40:28:5e:c9:8f:39:b8:5b:df:81:89:8f:13:cc:68:
    93:6d:64:58:20:3c:0a:82:ce:ec:2f:9b:b2:9d:ca:
    e7:19:22:98:29:6e:7c:4d:85:45:17:50:8f:5d:b1:
    45:be:42:af:1a:7f:84:26:b4:5d:a6:22:8a:07:e8:
    b3:b4:5a:59:45:20:b5:ef:1c:81:25:9e:73:74:04:
    d6:57:30:2c:a7:25:50:7c:d7:87:73:b3:d0:c2:8b:
    c9:02:8e:15:9e:40:41:a5:7a:a9:d8:85:fb:5b:9a:
    59:83:bc:80:fa:74:e6:88:14:70:33:61:d7:f5:51:
    47:8f:60:51:cb:c4:97:66:65:94:f0:ed:58:ca:80:
    c1:89:e0:55:68:4c:69:21:0f:08:27:e0:87:11:df:
    b7:bd
prime1:
    00:f7:ff:b0:40:de:62:b6:a2:e5:d0:f5:fa:28:3d:
    d3:30:30:89:8f:d1:ae:df:e9:09:ee:a0:b0:a5:a5:
    a4:e5:93:97:7e:e6:0b:09:70:4c:62:99:5e:7d:45:
    2f:fd:21:5a:31:d9:26:7f:39:5f:6e:eb:36:02:4e:
    18:99:1b:38:13:99:f5:f3:a3:6b:93:83:67:fb:58:
    67:d4:07:eb:e3:2f:31:b3:97:8f:f6:86:1f:15:08:
    1a:4b:b5:a8:06:97:72:9c:74:ab:53:1f:ac:ee:fb:
    59:03:39:a6:5c:a8:77:43:c0:2c:14:60:0e:71:3d:
    70:b6:59:09:40:86:04:54:bf
prime2:
    00:da:f0:73:2c:bd:52:a5:0d:9a:40:c4:34:fc:c9:
    cf:0f:67:8a:02:01:ca:e7:b8:4e:57:da:0c:0d:b2:
    f9:f3:f2:e4:4c:82:61:aa:04:2c:88:39:18:bd:86:
    d6:dc:d0:e9:6c:c6:6f:d9:87:59:57:9b:1a:6b:c9:
    56:c1:4d:33:ce:3e:15:b9:42:4e:e0:f8:14:91:c3:
    fe:63:b2:13:29:99:a7:a6:13:cc:f8:9c:38:29:28:
    dd:ed:d1:a3:7c:05:2c:26:a0:84:c6:09:9e:42:ef:
    7b:5e:50:c7:57:e3:bc:02:93:0b:74:a1:b5:0b:6e:
    23:18:8b:82:6f:ac:3c:0b:6f
exponent1:
    7c:a1:23:4b:46:37:27:7f:6f:ac:f6:a0:93:ae:96:
    3e:46:76:2b:2f:7e:09:8a:8c:72:3e:90:e7:7d:fa:
    03:61:8b:a5:bb:27:da:c3:73:af:ad:51:9d:f4:b2:
    2c:2c:a1:ae:21:69:c6:4f:e7:d4:cf:21:a2:40:ea:
    fd:ae:7f:1c:e2:a7:86:9c:1e:c8:d0:25:e6:5b:44:
    3a:7b:0c:a1:6c:2b:37:0c:b8:cd:74:13:94:b7:30:
    b7:d1:7f:b2:68:53:b1:aa:b4:1a:9e:f5:82:58:10:
    20:9d:cd:2c:0d:81:7a:2b:ce:3b:23:16:be:f3:d8:
    7b:da:fc:da:4f:3f:47:f3
exponent2:
    66:c9:5c:49:34:d9:08:04:4a:d6:fd:46:a3:27:5b:
    be:af:ad:6b:23:cc:4e:dd:88:6a:56:44:32:6a:44:
    4e:f3:49:9b:61:da:d8:26:fd:81:36:cd:16:ad:a7:
    52:24:02:72:be:f6:e3:f9:57:48:79:d8:fd:a1:98:
    c9:47:a5:7a:be:4b:14:9e:bc:c9:81:ae:a6:80:8d:
    7d:e0:ac:7e:6b:54:f9:f3:71:d7:86:00:17:d2:c7:
    de:4e:fd:a1:cc:0b:de:56:9d:ff:1b:a4:e1:67:ed:
    53:6a:39:2c:5a:0e:7a:66:ee:89:e3:21:4c:2c:78:
    ed:9d:11:af:bb:fc:b4:a1
coefficient:
    00:b1:23:a8:cc:b1:5e:2e:38:09:0c:b5:df:2c:c6:
    15:e8:08:48:45:b9:9d:ec:6f:27:45:5b:a7:bc:b6:
    b1:ec:a5:39:b4:40:8e:bc:40:1f:b9:4d:14:2e:18:
    fb:87:1e:20:91:34:58:e3:ac:c3:4a:dc:a8:2a:97:
    ce:aa:8d:62:0e:91:af:1f:53:d6:37:55:1d:14:9c:
    01:98:34:77:28:d7:cf:f7:a0:2d:73:40:48:5e:ed:
    ae:9b:15:42:06:e6:a3:5a:2b:b0:bc:ee:7a:bb:52:
    e6:28:19:c2:e5:de:6f:4d:fa:fb:69:81:7b:13:2b:
    01:87:bf:bf:66:8f:24:a1:8f
生成根证书请求
Enter pass phrase for ca/private/ca.pem:
查看证书请求
Certificate Request:
    Data:
        Version: 0 (0x0)
        Subject: C=CN, ST=BJ, L=BJ, O=zlex, OU=zlex, CN=ca.zlex.org
        Subject Public Key Info:
            Public Key Algorithm: rsaEncryption
            RSA Public Key: (2048 bit)
                Modulus (2048 bit):
                    00:d4:18:ab:5f:ad:b7:d0:09:d4:68:63:b5:db:8a:
                    d1:a1:db:7e:f3:bb:bb:c2:be:a7:35:17:9e:bb:20:
                    d3:1f:ed:63:e7:7d:29:6d:d2:7c:60:06:47:53:a6:
                    23:b0:bd:94:65:3f:57:1e:00:51:f3:a1:9a:1b:83:
                    14:a5:53:72:86:21:a2:57:22:2f:6a:a9:46:50:8c:
                    f0:51:cf:e6:83:5b:23:dc:f9:ea:6c:2e:51:20:61:
                    d1:84:9f:28:e8:01:89:b5:cb:55:68:4a:11:b1:06:
                    56:31:21:16:c8:ac:2b:68:31:e1:de:12:d3:21:12:
                    83:36:4c:ca:a8:b5:7e:b9:a7:63:4e:8e:e0:79:0f:
                    0e:91:36:28:7c:dd:9a:e2:e0:98:8b:91:7f:09:7d:
                    20:bb:37:f2:ab:aa:f0:ef:ae:68:7e:db:ca:db:33:
                    84:48:5a:e3:ff:0b:08:0e:96:6d:01:c8:12:35:ec:
                    9f:31:55:7f:53:7e:bd:fb:c4:16:b8:1f:17:29:42:
                    0f:0e:04:57:14:18:fd:e5:d6:3f:40:04:cd:85:dd:
                    d3:eb:2f:9a:bf:3c:8a:60:01:88:2f:43:0a:8b:bb:
                    50:13:f8:cc:68:f9:10:eb:f9:7e:63:de:62:55:32:
                    a8:fe:ce:51:67:79:c9:a6:3b:a3:c9:d7:81:7c:48:
                    f3:d1
                Exponent: 65537 (0x10001)
        Attributes:
            a0:00
    Signature Algorithm: sha1WithRSAEncryption
        af:91:f8:56:6f:db:de:cb:df:2c:87:93:99:ac:4b:51:12:a2:
        c1:2b:09:d2:58:7c:e1:07:5c:53:9f:f3:e1:b6:3a:e9:08:e7:
        65:89:3b:0a:01:83:24:a3:b5:74:65:50:a5:77:bc:30:1b:7d:
        80:8b:4c:92:ec:81:91:6e:b7:8f:05:e7:1d:b2:89:84:18:8c:
        5f:66:be:19:15:ba:ba:c3:f7:0d:c3:7d:7a:11:47:17:e5:cf:
        87:69:2e:15:91:d7:db:9d:8e:c9:0f:81:71:fa:00:93:33:2c:
        99:e1:be:76:06:f1:8a:e6:8b:1d:9b:07:70:f0:f2:44:91:ed:
        a2:ed:28:91:5f:6a:8a:f3:cf:ab:0d:b3:05:30:72:19:86:ae:
        c6:2d:a4:22:9f:21:cf:55:0c:b7:79:44:01:6e:36:43:a5:dc:
        a0:ea:46:2a:b0:9d:b3:53:4a:57:fc:72:1b:4c:52:cc:a3:39:
        d6:49:d6:f4:8c:e2:bf:5a:a6:6e:69:7c:f2:bc:7b:02:b7:f5:
        91:7f:94:2b:8c:58:0f:aa:a3:72:93:46:fe:08:29:08:51:eb:
        c6:a0:4e:7a:e1:bd:c6:0b:11:9d:63:96:af:22:ee:7b:79:84:
        cd:e7:f0:23:17:e7:9f:a2:73:c5:15:e1:f5:a1:af:8d:58:f5:
        e0:eb:57:fd
签发根证书
Using configuration from /etc/pki/tls/openssl.cnf
Enter pass phrase for ca/private/ca.pem:
Check that the request matches the signature
Signature ok
Certificate Details:
        Serial Number: 1 (0x1)
        Validity
            Not Before: Jul 24 08:15:59 2012 GMT
            Not After : Jul 22 08:15:59 2022 GMT
        Subject:
            countryName               = CN
            stateOrProvinceName       = BJ
            organizationName          = zlex
            organizationalUnitName    = zlex
            commonName                = ca.zlex.org
        X509v3 extensions:
            X509v3 Subject Key Identifier:
                7E:C9:9A:37:37:66:AC:79:41:63:F0:61:48:CD:24:39:2F:C2:0E:E9
            X509v3 Authority Key Identifier:
                keyid:7E:C9:9A:37:37:66:AC:79:41:63:F0:61:48:CD:24:39:2F:C2:0E:E9
                DirName:/C=CN/ST=BJ/O=zlex/OU=zlex/CN=ca.zlex.org
                serial:01
            X509v3 Basic Constraints:
                CA:TRUE
Certificate is to be certified until Jul 22 08:15:59 2022 GMT (3650 days)
Write out database with 1 new entries
Data Base Updated
查看证书详情
Certificate:
    Data:
        Version: 3 (0x2)
        Serial Number: 1 (0x1)
        Signature Algorithm: sha1WithRSAEncryption
        Issuer: C=CN, ST=BJ, O=zlex, OU=zlex, CN=ca.zlex.org
        Validity
            Not Before: Jul 24 08:15:59 2012 GMT
            Not After : Jul 22 08:15:59 2022 GMT
        Subject: C=CN, ST=BJ, O=zlex, OU=zlex, CN=ca.zlex.org
        Subject Public Key Info:
            Public Key Algorithm: rsaEncryption
            RSA Public Key: (2048 bit)
                Modulus (2048 bit):
                    00:d4:18:ab:5f:ad:b7:d0:09:d4:68:63:b5:db:8a:
                    d1:a1:db:7e:f3:bb:bb:c2:be:a7:35:17:9e:bb:20:
                    d3:1f:ed:63:e7:7d:29:6d:d2:7c:60:06:47:53:a6:
                    23:b0:bd:94:65:3f:57:1e:00:51:f3:a1:9a:1b:83:
                    14:a5:53:72:86:21:a2:57:22:2f:6a:a9:46:50:8c:
                    f0:51:cf:e6:83:5b:23:dc:f9:ea:6c:2e:51:20:61:
                    d1:84:9f:28:e8:01:89:b5:cb:55:68:4a:11:b1:06:
                    56:31:21:16:c8:ac:2b:68:31:e1:de:12:d3:21:12:
                    83:36:4c:ca:a8:b5:7e:b9:a7:63:4e:8e:e0:79:0f:
                    0e:91:36:28:7c:dd:9a:e2:e0:98:8b:91:7f:09:7d:
                    20:bb:37:f2:ab:aa:f0:ef:ae:68:7e:db:ca:db:33:
                    84:48:5a:e3:ff:0b:08:0e:96:6d:01:c8:12:35:ec:
                    9f:31:55:7f:53:7e:bd:fb:c4:16:b8:1f:17:29:42:
                    0f:0e:04:57:14:18:fd:e5:d6:3f:40:04:cd:85:dd:
                    d3:eb:2f:9a:bf:3c:8a:60:01:88:2f:43:0a:8b:bb:
                    50:13:f8:cc:68:f9:10:eb:f9:7e:63:de:62:55:32:
                    a8:fe:ce:51:67:79:c9:a6:3b:a3:c9:d7:81:7c:48:
                    f3:d1
                Exponent: 65537 (0x10001)
        X509v3 extensions:
            X509v3 Subject Key Identifier:
                7E:C9:9A:37:37:66:AC:79:41:63:F0:61:48:CD:24:39:2F:C2:0E:E9
            X509v3 Authority Key Identifier:
                keyid:7E:C9:9A:37:37:66:AC:79:41:63:F0:61:48:CD:24:39:2F:C2:0E:E9
                DirName:/C=CN/ST=BJ/O=zlex/OU=zlex/CN=ca.zlex.org
                serial:01
            X509v3 Basic Constraints:
                CA:TRUE
    Signature Algorithm: sha1WithRSAEncryption
        8a:99:b8:17:fc:64:7b:88:9c:1b:91:23:60:f4:5c:51:16:9a:
        9f:42:b4:d3:a5:bb:79:ca:78:e3:fc:a7:af:66:da:ec:5a:8c:
        81:c1:aa:04:32:a9:59:e0:d6:6a:f2:37:38:97:70:a5:27:5d:
        14:73:2e:2d:73:78:1d:37:2c:04:f7:c3:99:9d:be:0c:dd:2a:
        27:2c:0f:6e:95:96:01:c7:4c:99:f7:49:69:f9:ba:cb:62:b8:
        c6:43:6c:5b:b5:cd:25:42:a7:fb:81:27:bc:d8:e4:95:26:7d:
        da:50:f8:b8:be:0a:3d:54:35:d0:9d:22:e7:f0:f0:4c:7d:b4:
        57:2e:98:91:1a:1d:49:e5:8e:48:f6:2b:54:7e:04:fc:1c:e3:
        52:f7:04:f6:9b:bb:84:25:31:f7:31:6e:7f:fa:4c:e4:15:a2:
        86:0a:1a:56:8c:ad:07:49:fb:bc:28:27:a3:95:ba:eb:b3:28:
        db:11:78:ef:84:fc:3c:16:df:58:39:2e:14:8d:89:fe:7a:d2:
        24:eb:a7:66:11:8c:88:55:40:e1:c3:3b:95:b2:bc:af:36:0e:
        92:a8:cd:62:d5:57:9c:11:1b:f6:a1:36:5f:25:6c:16:c5:e2:
        68:19:e7:12:3d:4b:07:24:81:e6:71:f9:59:c5:f9:1c:62:6d:
        b3:24:b9:8a
证书转换——根证书
Enter pass phrase for ca/private/ca.pem:
Enter Export Password:
Verifying - Enter Export Password:
生成服务器端私钥
Generating RSA private key, 1024 bit long modulus
......................................................++++++
................++++++
e is 65537 (0x10001)
Enter pass phrase for ca/private/server.pem:
Verifying - Enter pass phrase for ca/private/server.pem:
查看私钥信息
Enter pass phrase for ca/private/server.pem:
Private-Key: (1024 bit)
modulus:
    00:d8:f9:bd:0a:a8:d3:97:98:b2:22:af:29:a9:31:
    76:50:52:77:c8:3b:7c:91:75:db:b3:63:88:cc:00:
    be:1a:6c:e6:80:23:90:37:5f:1a:d3:80:f2:7f:b5:
    77:01:ec:85:3e:4e:c0:af:0d:77:c0:a5:8b:bc:c3:
    fe:70:91:66:17:a4:ec:23:08:5b:e3:df:a3:40:2f:
    e6:83:bd:3f:d0:62:9c:c0:36:ad:e7:cb:13:e8:34:
    d7:6a:66:57:f5:bb:94:2f:7c:d5:27:7b:ee:e6:4f:
    fc:ff:c1:a4:01:96:d6:a0:b8:46:1d:93:02:a6:c5:
    00:bd:d9:e9:4e:2d:87:d5:95
publicExponent: 65537 (0x10001)
privateExponent:
    4d:da:15:fd:6c:24:37:c1:bf:30:f8:be:af:09:a3:
    55:20:b1:ff:f3:70:37:d5:1d:16:99:c1:2c:c9:9b:
    6c:69:e4:ae:d7:93:d8:7a:54:6a:cd:5a:b5:7e:0c:
    0c:71:ac:41:76:0a:67:05:23:11:c9:94:81:0f:a6:
    0d:07:ee:a4:26:0e:20:ff:36:6c:f7:2d:fa:8e:39:
    85:f8:b8:1a:e0:be:26:f8:24:3c:d4:d0:a0:89:9c:
    48:15:d9:28:de:51:dd:14:3f:ca:c9:63:ed:5d:e4:
    50:b0:06:5e:1b:f8:99:b4:49:f6:d6:cb:60:8a:7b:
    fa:f8:6e:86:44:55:e5:45
prime1:
    00:ef:cc:38:ab:e6:98:71:09:32:5c:69:b3:e0:59:
    9d:d7:7a:f9:e3:b9:cd:a8:84:74:1a:91:2a:db:2c:
    96:40:5a:28:0b:99:6c:da:fa:ca:83:54:e0:59:06:
    84:df:55:9a:04:9c:1c:6b:54:52:d5:31:d7:f9:0e:
    9a:13:b0:ed:03
prime2:
    00:e7:a2:c3:03:55:d7:54:7c:3a:38:40:f1:ac:9a:
    e8:dd:3a:5c:24:a6:78:34:c4:ce:24:c8:31:de:5a:
    0e:df:09:df:7c:ad:36:14:e0:be:6d:2c:58:89:c6:
    7e:ec:51:82:68:81:91:ed:b5:04:ff:c0:61:8e:aa:
    5b:ee:6b:f3:87
exponent1:
    2a:22:0c:d7:0f:56:3b:8e:2d:1e:15:a8:78:43:e6:
    ba:e4:ad:a1:78:95:0d:05:f0:cc:76:33:3c:7d:52:
    0d:0e:8a:38:b7:85:6b:d8:62:da:be:80:08:c4:5f:
    76:4a:39:1c:94:3d:5e:12:5b:d7:7f:c1:7d:ce:35:
    fe:3d:b8:f7
exponent2:
    00:94:0b:ec:36:52:84:19:04:79:35:81:14:b5:ec:
    20:8f:5d:00:8d:90:34:5e:0d:b7:6f:bc:e0:5a:ac:
    16:bb:29:15:45:1b:73:e8:6e:28:67:a0:a3:4a:13:
    ab:05:a1:a7:06:e2:61:81:9b:64:01:8e:55:0c:19:
    08:3e:df:92:3b
coefficient:
    00:8e:4e:ee:04:55:cc:4f:0f:c0:02:a4:9d:08:a8:
    4b:ec:72:7c:86:27:a9:0a:5e:1c:94:65:9e:c6:8a:
    6a:5c:9b:76:5d:c0:ae:f8:36:61:15:3b:67:fb:15:
    b3:cf:f4:2c:9b:56:66:13:89:89:69:01:d9:6e:b0:
    f7:02:d4:06:c9
生成服务器端证书请求
Enter pass phrase for ca/private/server.pem:
查看证书请求
Certificate Request:
    Data:
        Version: 0 (0x0)
        Subject: C=CN, ST=BJ, L=BJ, O=zlex, OU=zlex, CN=www.zlex.org
        Subject Public Key Info:
            Public Key Algorithm: rsaEncryption
            RSA Public Key: (1024 bit)
                Modulus (1024 bit):
                    00:d8:f9:bd:0a:a8:d3:97:98:b2:22:af:29:a9:31:
                    76:50:52:77:c8:3b:7c:91:75:db:b3:63:88:cc:00:
                    be:1a:6c:e6:80:23:90:37:5f:1a:d3:80:f2:7f:b5:
                    77:01:ec:85:3e:4e:c0:af:0d:77:c0:a5:8b:bc:c3:
                    fe:70:91:66:17:a4:ec:23:08:5b:e3:df:a3:40:2f:
                    e6:83:bd:3f:d0:62:9c:c0:36:ad:e7:cb:13:e8:34:
                    d7:6a:66:57:f5:bb:94:2f:7c:d5:27:7b:ee:e6:4f:
                    fc:ff:c1:a4:01:96:d6:a0:b8:46:1d:93:02:a6:c5:
                    00:bd:d9:e9:4e:2d:87:d5:95
                Exponent: 65537 (0x10001)
        Attributes:
            a0:00
    Signature Algorithm: sha1WithRSAEncryption
        2b:e9:b9:0b:e0:94:56:95:dd:59:1e:19:16:e0:f9:73:db:50:
        63:d3:d4:4d:5c:9b:98:9f:a7:6d:9b:4d:ae:67:52:18:e1:42:
        b0:66:7c:75:6a:db:98:bc:e6:47:08:aa:55:ca:ce:35:5c:5a:
        60:8b:7b:c8:f0:10:8a:bd:5f:d7:c8:b8:48:03:18:7e:68:6e:
        69:35:9c:c8:b0:c8:65:43:43:25:35:d7:d2:70:45:55:ab:78:
        51:4d:22:c3:68:b2:97:b5:3c:86:e8:2b:43:de:5d:e4:b0:b5:
        0e:eb:84:9d:42:81:ee:e0:0a:48:40:6a:93:a4:bd:3a:45:6f:
        20:24
签发服务器端证书
Using configuration from /etc/pki/tls/openssl.cnf
Enter pass phrase for ca/private/ca.pem:
Check that the request matches the signature
Signature ok
Certificate Details:
        Serial Number: 2 (0x2)
        Validity
            Not Before: Jul 24 08:16:15 2012 GMT
            Not After : Jul 24 08:16:15 2013 GMT
        Subject:
            countryName               = CN
            stateOrProvinceName       = BJ
            organizationName          = zlex
            organizationalUnitName    = zlex
            commonName                = www.zlex.org
        X509v3 extensions:
            X509v3 Basic Constraints:
                CA:FALSE
            Netscape Comment:
                OpenSSL Generated Certificate
            X509v3 Subject Key Identifier:
                CF:79:10:96:42:84:0C:51:DE:6E:DB:3C:5B:08:F1:E1:EB:0C:26:B9
            X509v3 Authority Key Identifier:
                keyid:7E:C9:9A:37:37:66:AC:79:41:63:F0:61:48:CD:24:39:2F:C2:0E:E9
Certificate is to be certified until Jul 24 08:16:15 2013 GMT (365 days)
Sign the certificate? [y/n]:y
1 out of 1 certificate requests certified, commit? [y/n]y
Write out database with 1 new entries
Data Base Updated
查看证书详情
Certificate:
    Data:
        Version: 3 (0x2)
        Serial Number: 2 (0x2)
        Signature Algorithm: sha1WithRSAEncryption
        Issuer: C=CN, ST=BJ, O=zlex, OU=zlex, CN=ca.zlex.org
        Validity
            Not Before: Jul 24 08:16:15 2012 GMT
            Not After : Jul 24 08:16:15 2013 GMT
        Subject: C=CN, ST=BJ, O=zlex, OU=zlex, CN=www.zlex.org
        Subject Public Key Info:
            Public Key Algorithm: rsaEncryption
            RSA Public Key: (1024 bit)
                Modulus (1024 bit):
                    00:d8:f9:bd:0a:a8:d3:97:98:b2:22:af:29:a9:31:
                    76:50:52:77:c8:3b:7c:91:75:db:b3:63:88:cc:00:
                    be:1a:6c:e6:80:23:90:37:5f:1a:d3:80:f2:7f:b5:
                    77:01:ec:85:3e:4e:c0:af:0d:77:c0:a5:8b:bc:c3:
                    fe:70:91:66:17:a4:ec:23:08:5b:e3:df:a3:40:2f:
                    e6:83:bd:3f:d0:62:9c:c0:36:ad:e7:cb:13:e8:34:
                    d7:6a:66:57:f5:bb:94:2f:7c:d5:27:7b:ee:e6:4f:
                    fc:ff:c1:a4:01:96:d6:a0:b8:46:1d:93:02:a6:c5:
                    00:bd:d9:e9:4e:2d:87:d5:95
                Exponent: 65537 (0x10001)
        X509v3 extensions:
            X509v3 Basic Constraints:
                CA:FALSE
            Netscape Comment:
                OpenSSL Generated Certificate
            X509v3 Subject Key Identifier:
                CF:79:10:96:42:84:0C:51:DE:6E:DB:3C:5B:08:F1:E1:EB:0C:26:B9
            X509v3 Authority Key Identifier:
                keyid:7E:C9:9A:37:37:66:AC:79:41:63:F0:61:48:CD:24:39:2F:C2:0E:E9
    Signature Algorithm: sha1WithRSAEncryption
        3d:85:0a:f5:a6:8e:f5:13:1b:fc:74:b6:50:8f:fe:0c:e6:32:
        0e:0c:5a:0a:75:2d:e8:15:39:2f:93:46:29:c6:cc:27:5a:36:
        a0:93:f8:bc:38:d5:2d:55:b9:19:de:81:6f:b6:5f:1f:07:65:
        81:c5:12:4e:ea:3e:09:d0:d5:b8:66:c1:cd:4d:5d:51:19:a1:
        7f:7b:cb:dc:bf:b0:be:3e:f8:8b:74:d3:31:a9:95:a3:ef:25:
        a3:1e:98:65:0f:d4:40:51:ef:42:02:72:f0:59:26:8a:e7:d6:
        ca:34:ad:fb:3d:a8:e7:05:93:a6:78:bd:b5:90:51:83:06:2b:
        95:db:01:0c:89:9f:74:a4:32:89:c5:15:c6:ec:e2:61:10:29:
        70:da:c5:ea:d6:9c:be:c3:4c:a1:42:6a:26:2f:23:7c:90:51:
        8f:51:ee:49:c9:6b:9c:0c:15:a2:d3:dc:90:19:db:4d:d1:ad:
        ca:06:d1:e1:60:20:18:b1:6d:0b:17:f7:06:e6:e8:d1:b0:0c:
        6d:55:16:f1:63:54:da:c2:3f:6c:e5:99:68:7a:a0:fa:29:5c:
        dc:cf:34:90:fb:91:7b:e0:5d:bb:a0:9d:91:f3:17:bd:0b:5a:
        69:d7:0c:24:75:ca:b2:08:da:bf:67:35:ce:01:d0:4e:45:81:
        97:bd:fb:87
证书转换——服务器端
Enter pass phrase for ca/private/server.pem:
Enter Export Password:
Verifying - Enter Export Password:
生成客户端私钥
Generating RSA private key, 1024 bit long modulus
..++++++
...........++++++
e is 65537 (0x10001)
Enter pass phrase for ca/private/client.pem:
Verifying - Enter pass phrase for ca/private/client.pem:
查看私钥信息
Enter pass phrase for ca/private/client.pem:
Private-Key: (1024 bit)
modulus:
    00:b4:e9:7d:3d:6b:8b:07:94:7d:47:51:56:3e:0e:
    92:2f:87:8c:60:0f:b8:cb:eb:90:6d:13:76:51:75:
    e4:3e:b7:6e:1f:f0:63:5b:f7:ba:51:c0:04:1e:f1:
    d0:ef:58:4a:35:47:4a:1a:11:72:fc:e9:10:82:ec:
    3e:0d:ef:7d:17:a0:5e:93:b4:01:8f:a5:27:3c:3e:
    a9:26:f0:00:ba:ca:24:98:92:51:3e:4b:d0:81:a7:
    fc:14:e2:98:f5:27:f2:51:4c:a8:ae:b4:5f:e7:cc:
    70:7e:23:57:92:6a:cf:d4:1d:6f:b3:52:8a:4a:1a:
    1b:65:f0:4d:1c:0b:1f:50:eb
publicExponent: 65537 (0x10001)
privateExponent:
    3a:35:b2:8d:73:af:fd:55:62:e5:f2:9e:dc:42:d5:
    f8:a3:15:a0:c7:0e:3f:d6:e0:d6:a7:df:77:20:86:
    bb:43:4c:14:cc:c5:3b:8f:3f:0d:14:ca:7e:a6:72:
    02:c1:16:c7:83:d3:ad:05:96:49:18:38:ae:d7:92:
    b3:eb:2e:05:43:d6:3d:04:3c:0b:fc:15:79:c5:85:
    10:ed:21:6e:30:73:0b:a6:4f:9a:fe:db:4a:98:bc:
    ec:03:7b:7f:e6:16:2f:a5:f3:5e:0d:cf:ce:eb:4a:
    3e:c5:b9:7f:fc:4c:60:9e:0e:d4:aa:91:5a:46:f7:
    b3:77:fc:0b:1b:62:70:b9
prime1:
    00:ef:6d:7f:92:6a:af:21:59:ed:fe:49:a8:7c:4a:
    1d:4d:7c:f9:38:bf:e7:dc:42:41:e1:33:f9:e1:c7:
    74:45:2e:1c:e4:40:8d:5f:1a:ac:11:9e:a4:6c:1d:
    00:6d:4e:aa:4d:58:e9:92:84:ac:d9:29:67:e0:79:
    a8:a3:15:e3:2d
prime2:
    00:c1:6f:21:c5:62:48:78:3a:0f:25:98:00:46:d6:
    c2:2d:0f:96:fb:20:4b:f4:03:81:71:3f:6f:30:c0:
    f3:a6:e6:f4:00:a4:fa:0b:97:e6:2a:21:8c:cb:c1:
    28:eb:5f:f6:01:62:85:9a:37:98:e7:53:a4:8b:3f:
    bd:77:eb:f3:77
exponent1:
    00:e3:71:e0:9b:85:af:22:7e:9c:a0:50:f6:b6:43:
    6d:bc:bb:b8:c0:d9:44:f8:2f:15:08:4b:68:d8:bb:
    b1:cf:3a:34:05:fc:f0:8f:64:f6:0a:b2:ea:bd:2d:
    7b:c7:5a:d0:5b:33:d8:86:f0:74:86:c3:57:c3:9d:
    ae:be:66:3f:6d
exponent2:
    00:82:4a:c9:04:9b:5f:15:1c:86:77:5c:1b:53:9b:
    f4:cf:45:60:fd:66:93:c2:99:59:e7:5e:43:17:23:
    e0:fa:db:36:1f:f9:00:34:2e:ec:ea:14:0f:32:6f:
    b9:90:51:e2:f2:ab:da:32:36:a0:d7:b0:8f:74:fc:
    4a:33:2c:cb:a1
coefficient:
    51:c1:7e:d7:0d:98:86:cb:ca:41:ea:aa:54:6c:00:
    49:c3:18:12:c4:5b:75:fe:0d:0c:e2:2f:0f:93:8e:
    8e:01:c5:9d:ff:40:2b:20:08:24:7f:a5:f2:da:67:
    96:5e:e6:7e:1e:52:32:2f:88:ef:df:20:6a:75:ec:
    28:cd:fa:a0
生成客户端证书请求
Enter pass phrase for ca/private/client.pem:
查看证书请求
Certificate Request:
    Data:
        Version: 0 (0x0)
        Subject: C=CN, ST=BJ, L=BJ, O=zlex, OU=zlex, CN=zlex
        Subject Public Key Info:
            Public Key Algorithm: rsaEncryption
            RSA Public Key: (1024 bit)
                Modulus (1024 bit):
                    00:b4:e9:7d:3d:6b:8b:07:94:7d:47:51:56:3e:0e:
                    92:2f:87:8c:60:0f:b8:cb:eb:90:6d:13:76:51:75:
                    e4:3e:b7:6e:1f:f0:63:5b:f7:ba:51:c0:04:1e:f1:
                    d0:ef:58:4a:35:47:4a:1a:11:72:fc:e9:10:82:ec:
                    3e:0d:ef:7d:17:a0:5e:93:b4:01:8f:a5:27:3c:3e:
                    a9:26:f0:00:ba:ca:24:98:92:51:3e:4b:d0:81:a7:
                    fc:14:e2:98:f5:27:f2:51:4c:a8:ae:b4:5f:e7:cc:
                    70:7e:23:57:92:6a:cf:d4:1d:6f:b3:52:8a:4a:1a:
                    1b:65:f0:4d:1c:0b:1f:50:eb
                Exponent: 65537 (0x10001)
        Attributes:
            a0:00
    Signature Algorithm: sha1WithRSAEncryption
        91:5b:b2:2e:b3:54:14:92:7a:44:c0:59:11:0f:fe:08:50:33:
        09:0f:73:d3:9d:15:43:07:66:4a:9e:7c:de:12:4d:bc:b6:3a:
        7a:6b:36:40:3a:4b:ea:db:f7:2e:a1:de:ce:4f:a6:98:14:3b:
        c0:f6:3d:fe:db:82:fa:c7:f1:1e:9a:6c:2b:ff:e6:a4:91:b1:
        ab:20:44:91:a8:d9:1b:13:8f:9e:24:68:16:f3:c1:66:7b:3b:
        29:b5:61:3d:be:88:00:d8:0a:1c:63:f0:25:6c:33:7d:86:80:
        54:d5:75:db:6f:7e:9c:52:4c:70:0d:5a:88:ae:b5:1a:12:41:
        e4:47
签发客户端证书
Using configuration from /etc/pki/tls/openssl.cnf
Enter pass phrase for ca/private/ca.pem:
Check that the request matches the signature
Signature ok
Certificate Details:
        Serial Number: 3 (0x3)
        Validity
            Not Before: Jul 24 08:16:35 2012 GMT
            Not After : Jul 24 08:16:35 2013 GMT
        Subject:
            countryName               = CN
            stateOrProvinceName       = BJ
            organizationName          = zlex
            organizationalUnitName    = zlex
            commonName                = zlex
        X509v3 extensions:
            X509v3 Basic Constraints:
                CA:FALSE
            Netscape Comment:
                OpenSSL Generated Certificate
            X509v3 Subject Key Identifier:
                FD:85:1C:BA:E0:C4:81:F5:F4:92:F1:FC:8A:59:77:33:60:6F:47:F7
            X509v3 Authority Key Identifier:
                keyid:7E:C9:9A:37:37:66:AC:79:41:63:F0:61:48:CD:24:39:2F:C2:0E:E9
Certificate is to be certified until Jul 24 08:16:35 2013 GMT (365 days)
Sign the certificate? [y/n]:y
1 out of 1 certificate requests certified, commit? [y/n]y
Write out database with 1 new entries
Data Base Updated
查看证书详情
Certificate:
    Data:
        Version: 3 (0x2)
        Serial Number: 3 (0x3)
        Signature Algorithm: sha1WithRSAEncryption
        Issuer: C=CN, ST=BJ, O=zlex, OU=zlex, CN=ca.zlex.org
        Validity
            Not Before: Jul 24 08:16:35 2012 GMT
            Not After : Jul 24 08:16:35 2013 GMT
        Subject: C=CN, ST=BJ, O=zlex, OU=zlex, CN=zlex
        Subject Public Key Info:
            Public Key Algorithm: rsaEncryption
            RSA Public Key: (1024 bit)
                Modulus (1024 bit):
                    00:b4:e9:7d:3d:6b:8b:07:94:7d:47:51:56:3e:0e:
                    92:2f:87:8c:60:0f:b8:cb:eb:90:6d:13:76:51:75:
                    e4:3e:b7:6e:1f:f0:63:5b:f7:ba:51:c0:04:1e:f1:
                    d0:ef:58:4a:35:47:4a:1a:11:72:fc:e9:10:82:ec:
                    3e:0d:ef:7d:17:a0:5e:93:b4:01:8f:a5:27:3c:3e:
                    a9:26:f0:00:ba:ca:24:98:92:51:3e:4b:d0:81:a7:
                    fc:14:e2:98:f5:27:f2:51:4c:a8:ae:b4:5f:e7:cc:
                    70:7e:23:57:92:6a:cf:d4:1d:6f:b3:52:8a:4a:1a:
                    1b:65:f0:4d:1c:0b:1f:50:eb
                Exponent: 65537 (0x10001)
        X509v3 extensions:
            X509v3 Basic Constraints:
                CA:FALSE
            Netscape Comment:
                OpenSSL Generated Certificate
            X509v3 Subject Key Identifier:
                FD:85:1C:BA:E0:C4:81:F5:F4:92:F1:FC:8A:59:77:33:60:6F:47:F7
            X509v3 Authority Key Identifier:
                keyid:7E:C9:9A:37:37:66:AC:79:41:63:F0:61:48:CD:24:39:2F:C2:0E:E9
    Signature Algorithm: sha1WithRSAEncryption
        b2:31:c0:15:a1:8f:2c:6d:61:0c:4f:6e:c1:fe:7a:88:e0:60:
        ce:6d:43:b4:29:d8:4d:83:4d:ea:ce:f0:8e:c1:c7:3b:bd:30:
        cb:92:71:11:7d:19:04:11:58:25:5d:1b:ed:6f:22:13:91:ea:
        13:7f:0e:99:00:ec:fb:b3:a5:e2:b9:ea:ea:bb:35:09:3b:ca:
        f5:49:ac:a1:d3:d5:ae:ff:ce:11:a9:2f:53:74:88:24:9f:f8:
        b2:bc:02:4d:1a:bb:c1:53:3e:6e:31:52:4d:ac:f8:14:bd:b1:
        0d:31:1d:aa:94:43:38:5e:fb:c2:26:3e:43:ba:25:3b:23:27:
        a8:7d:5d:3d:f9:97:28:71:51:1d:a4:56:44:b4:f6:51:4a:2b:
        8b:47:d3:10:49:04:cd:c3:58:62:75:bc:c7:6a:4c:d5:9a:a8:
        e9:9c:23:ec:f8:26:e5:de:43:4e:f2:8d:c2:75:40:70:3f:03:
        0f:74:78:7a:bc:ca:6f:90:a0:3e:3a:d2:92:16:d5:ca:af:93:
        28:1f:24:3a:7e:2c:b9:db:87:10:68:e0:c9:6c:0b:5d:9f:15:
        be:bc:13:22:af:7b:8f:e9:14:51:04:65:7a:69:18:c2:ca:4f:
        cb:e5:4c:62:41:88:b1:ee:ac:43:14:34:6d:58:af:52:b1:25:
        76:f3:0e:8f
证书转换——客户端
Enter pass phrase for ca/private/client.pem:
Enter Export Password:
Verifying - Enter Export Password:
生成证书链
查看证书链
subject=/C=CN/ST=BJ/O=zlex/OU=zlex/CN=www.zlex.org
issuer=/C=CN/ST=BJ/O=zlex/OU=zlex/CN=ca.zlex.org
subject=/C=CN/ST=BJ/O=zlex/OU=zlex/CN=ca.zlex.org
issuer=/C=CN/ST=BJ/O=zlex/OU=zlex/CN=ca.zlex.org
subject=/C=CN/ST=BJ/O=zlex/OU=zlex/CN=zlex
issuer=/C=CN/ST=BJ/O=zlex/OU=zlex/CN=ca.zlex.org
来看一下这3套证书，如下两幅图所示：
CA证书
![]()
![]()
![]()
服务器证书
![]()
![]()
![]()
客户证书
![]()
![]()
![]()
证书链
![]()
"ca.zlex.org"证书充当了CA根证书，"www.zlex.org"充当服务器端证书，"zlex"充当客户端证书
使用keytool将其导入本地密钥库
导入CA证书
Shell代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. keytool -import -v -trustcacerts -alias ca.zlex.org -file ca.crt -storepass 123456 -keystore ca.keystore          

keytool -import -v -trustcacerts -alias ca.zlex.org -file ca.crt -storepass 123456 -keystore ca.keystore       
控制台输出
引用

所有者:CN=ca.zlex.org, OU=zlex, O=zlex, L=BJ, ST=BJ, C=CN
签发人:CN=ca.zlex.org, OU=zlex, O=zlex, L=BJ, ST=BJ, C=CN
序列号:989b27ef00e53a99
有效期: Wed Jul 18 17:53:51 CST 2012 至Sat Jul 16 17:53:51 CST 2022
证书指纹:
         MD5:BA:14:1F:89:3A:1E:63:7B:20:AC:5A:50:FE:65:7E:16
         SHA1:E0:A4:0E:6F:09:7E:01:27:C0:FC:62:26:1A:0C:C6:7B:BF:6A:18:B3
         签名算法名称:SHA1withRSA
         版本: 1
信任这个认证？ [否]：  y
认证已添加至keystore中
[正在存储 ca.keystore]
导入服务器端证书
Shell代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. keytool -import -v -trustcacerts -alias www.zlex.org -file server.crt -storepass 123456 -keystore server.keystore  

keytool -import -v -trustcacerts -alias www.zlex.org -file server.crt -storepass 123456 -keystore server.keystore
控制台输出
引用

所有者:CN=www.zlex.org, OU=zlex, O=zlex, ST=BJ, C=CN
签发人:CN=ca.zlex.org, OU=zlex, O=zlex, L=BJ, ST=BJ, C=CN
序列号:1
有效期: Wed Jul 18 17:54:25 CST 2012 至Thu Jul 18 17:54:25 CST 2013
证书指纹:
         MD5:7E:5E:66:56:AF:E7:F5:72:0F:FC:95:85:97:07:4E:2A
         SHA1:B1:E7:E8:AC:AB:C9:72:69:D8:E2:25:D5:16:A9:AF:C1:B7:4A:74:5D
         签名算法名称:SHA1withRSA
         版本: 3
扩展:
#1: ObjectId: 2.5.29.14 Criticality=false
SubjectKeyIdentifier [
KeyIdentifier [
0000: A8 49 2F E2 2D 15 9F 42   BD 76 2B 20 D3 EB A5 EE  .I/.-..B.v+ ....
0010: 31 CA E7 63                                        1..c
]
]
#2: ObjectId: 2.5.29.19 Criticality=false
BasicConstraints:[
  CA:false
  PathLen: undefined
]
#3: ObjectId: 2.5.29.35 Criticality=false
AuthorityKeyIdentifier [
[CN=ca.zlex.org, OU=zlex, O=zlex, L=BJ, ST=BJ, C=CN]
SerialNumber: [    989b27ef 00e53a99]
]
#4: ObjectId: 2.16.840.1.113730.1.13 Criticality=false
信任这个认证？ [否]：  y
认证已添加至keystore中
[正在存储 server.keystore]
导入客户端证书
Shell代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. keytool -import -v -trustcacerts -alias client -file client.crt -storepass 123456 -keystore client.keystore  

keytool -import -v -trustcacerts -alias client -file client.crt -storepass 123456 -keystore client.keystore
以下是输出内容：
引用

所有者:CN=zlex, OU=zlex, O=zlex, ST=BJ, C=CN
签发人:CN=ca.zlex.org, OU=zlex, O=zlex, L=BJ, ST=BJ, C=CN
序列号:2
有效期: Wed Jul 18 17:54:49 CST 2012 至Thu Jul 18 17:54:49 CST 2013
证书指纹:
         MD5:81:16:ED:92:9E:17:DB:E3:BE:DE:CD:8D:F8:E0:EE:C4
         SHA1:C0:E0:42:81:79:70:4C:F8:44:D4:76:2D:C5:62:7C:67:B2:41:B3:AC
         签名算法名称:SHA1withRSA
         版本: 3
扩展:
#1: ObjectId: 2.5.29.14 Criticality=false
SubjectKeyIdentifier [
KeyIdentifier [
0000: 0C 2C 25 86 C6 8D 04 88   F5 63 19 DC 09 B1 3C 5D  .,%......c....<]
0010: 59 C9 72 1B                                        Y.r.
]
]
#2: ObjectId: 2.5.29.19 Criticality=false
BasicConstraints:[
  CA:false
  PathLen: undefined
]
#3: ObjectId: 2.5.29.35 Criticality=false
AuthorityKeyIdentifier [
[CN=ca.zlex.org, OU=zlex, O=zlex, L=BJ, ST=BJ, C=CN]
SerialNumber: [    989b27ef 00e53a99]
]
#4: ObjectId: 2.16.840.1.113730.1.13 Criticality=false
信任这个认证？ [否]：  y
认证已添加至keystore中
[正在存储 client.keystore]
PS 吊销证书：
Shell代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. echo 吊销客户端证书  
1. openssl ca -revoke $certs_path/client.crt -cert $certs_path/ca.crt -keyfile $private_path/ca.pem   

echo 吊销客户端证书

openssl ca -revoke $certs_path/client.crt -cert $certs_path/ca.crt -keyfile $private_path/ca.pem
引用

Using configuration from /etc/pki/tls/openssl.cnf
Enter pass phrase for private/ca.pem:
Revoking Certificate 02.
Data Base Updated
生成证书吊销列表文件（CRL）
执行命令如下：
Shell代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. openssl ca -gencrl -out ca.crl -config "$HOME/testca/conf/testca.conf"  

openssl ca -gencrl -out ca.crl -config "$HOME/testca/conf/testca.conf"
-crldays和-crlhours参数,说明下一个吊销列表将在多少天后（或多少小时候）发布。
可以用以下命令检查testca.crl的内容：
Shell代码 [![复制代码]()]( "复制代码") [![收藏代码]()![]()]( "收藏这段代码")

1. openssl crl -in testca.crl -text -noout  

openssl crl -in testca.crl -text -noout
引用
[http://blog.csdn.net/gothicane/articles/2865818.aspx](http://blog.csdn.net/gothicane/articles/2865818.aspx)
[http://www.5dlinux.com/article/7/2009/linux_35291.html](http://www.5dlinux.com/article/7/2009/linux_35291.html)
[http://www.tc.umn.edu/~brams006/selfsign_ubuntu.html](http://www.tc.umn.edu/~brams006/selfsign_ubuntu.html)
[http://www.tc.umn.edu/~brams006/selfsign.html](http://www.tc.umn.edu/~brams006/selfsign.html)
[http://zhouzhk.iteye.com/blog/136943](http://zhouzhk.iteye.com/blog/136943)
[http://bbs.cfan.com.cn/thread-743287-1-1.html](http://bbs.cfan.com.cn/thread-743287-1-1.html)
[http://www.iteye.com/problems/4072](http://www.iteye.com/problems/4072)
[http://blog.csdn.net/jasonhwang/archive/2008/04/26/2329589.aspx](http://blog.csdn.net/jasonhwang/archive/2008/04/26/2329589.aspx)
[http://blog.csdn.net/jasonhwang/archive/2008/04/29/2344768.aspx](http://blog.csdn.net/jasonhwang/archive/2008/04/29/2344768.aspx)
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
