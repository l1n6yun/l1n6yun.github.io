title: 自签名ssl证书
author: l1n6yun
tags: []
categories: []
date: 2024-07-13 22:24:00
---
**使用 OpenSSL 工具生成自签名 SSL 证书，为内网环境中的网站安全保驾护航**

在内网环境中，使用 OpenSSL 工具进行自签名 SSL 证书的创建，能够为您的网站提供有效的安全保障。

**自签证书流程**：

1. 创建 ca 私钥。
2. 基于 ca 私钥生成 ca 根证书。
3. 构建 ssl 私钥。
4. 生成 ssl 证书 csr。
5. 运用 ca 根证书签署以获得 ssl 证书。

**操作方法**

1. 创建 ca 私钥（强烈建议设置密码以增强安全性）：

```shell
$ openssl genrsa -des3 -out root.key 2048
Generating RSA private key, 2048 bit long modulus
..............................................................+++
................+++
e is 65537 (0x010001)
Enter pass phrase for root.key:

Verifying - Enter pass phrase for root.key:
```

2. 生成 ca 证书，自签 20 年有效期，并将此 ca 证书导入需要访问的 PC 的“受信任的根证书颁发机构”中，后续用此 ca 签署的证书均可正常使用：

```shell
$ openssl req -x509 -new -nodes -key root.key -sha256 -days 7300 -out root.crt
Enter pass phrase for root.key:

You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [AU]:CN    # 国家
State or Province Name (full name) [Some-State]:SiChuan   # 省份
Locality Name (eg, city) []:ChengDu   # 城市
Organization Name (eg, company) [Internet Widgits Pty Ltd]:Sichuan Lingyun Technology Co., LTD  # 组织
Organizational Unit Name (eg, section) []: # 部门
Common Name (e.g. server FQDN or YOUR name) []:Sichuan Lingyun Technology Co., LTD # 公司
Email Address []: # 邮箱
```

3. 完成上述步骤后，创建 ssl 证书私钥：


```shell
$ openssl genrsa -out server.key 2048
Generating RSA private key, 2048 bit long modulus
...................................................................................+++
..........+++
e is 65537 (0x010001)
```

4. 随后创建 ssl 证书 csr：

```shell
$ openssl req -new -key server.key -out server.csr
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [AU]:CN
State or Province Name (full name) [Some-State]:SiChuan
Locality Name (eg, city) []:ChengDu
Organization Name (eg, company) [Internet Widgits Pty Ltd]:Sichuan Lingyun Technology Co., LTD
Organizational Unit Name (eg, section) []:
Common Name (e.g. server FQDN or YOUR name) []:Sichuan Lingyun Technology Co., LTD
Email Address []:

Please enter the following 'extra' attributes
to be sent with your certificate request
A challenge password []:
An optional company name []:
```

5. 创建域名附加配置信息，新建一个文件，通过 vim cert.ext ，将以下代码粘贴后保存：

```ext
authorityKeyIdentifier=keyid,issuer
basicConstraints=CA:FALSE
keyUsage = digitalSignature, nonRepudiation, keyEncipherment, dataEncipherment
subjectAltName = @alt_names

[alt_names]
DNS.1 = localhost
IP.2 = 192.168.0.10
IP.3 = 192.168.1.200
DNS.4 = xa.it.com
DNS.5 = xiykj.com
DNS.6 = *.xa.com
```

需要注意的是，IP.2 = 192.168.11.100 表示 https 要访问的 IP 地址，IP.3 同理也是 IP 地址，ssl 证书允许自签多个 IP 地址，这便是自签 IP 的证书。而 DNS.4 = https://xa.it.com/ 则表示 https 要访问的域名，DNS.5、DNS.6 同理均为域名，ssl 证书也支持自签多个域名，此为自签域名的证书。

6. 使用CA根证书签署ssl证书，自签ssl证书有效期20年：

```shell
$ openssl x509 -req -in server.csr -out server.crt -days 7300 -CAcreateserial -CA root.crt -CAkey root.key -CAserial serial -extfile cert.ext
Signature ok
subject=C = CN, ST = SiChuan, L = ChengDu, O = "Sichuan Lingyun Technology Co., LTD"
Getting CA Private Key
Enter pass phrase for root.key:
```

7. 查看文件，通过执行“ls -al”命令

```shell
$ ls -al
cert.ext          #ssl证书附加配置信息
serial            #证书序列号
server.crt        #ssl证书文件，包含公钥信息
server.csr        #ssl证书签名文件
server.key        #ssl证书私钥
```

8. 查看签署的证书信息：


```shell
$ openssl x509 -in server.crt -noout -text
Certificate:
    Data:
        Version: 3 (0x2)
        Serial Number:
            cd:ae:a9:3e:b7:bb:93:e1
    Signature Algorithm: sha256WithRSAEncryption
        Issuer: C = CN, ST = SiChuan, L = ChengDu, O = "Sichuan Lingyun Technology Co., LTD"
        Validity
            Not Before: Jul 18 06:26:05 2024 GMT
            Not After : Jul 13 06:26:05 2044 GMT
        Subject: C = CN, ST = SiChuan, L = ChengDu, O = "Sichuan Lingyun Technology Co., LTD"
        Subject Public Key Info:
            Public Key Algorithm: rsaEncryption
                Public-Key: (2048 bit)
                Modulus:
                    00:aa:4e:ca:f5:0d:51:e7:b4:ba:3c:0b:6e:a5:4a:
                    d8:b4:0d:ad:19:54:3d:11:02:14:29:41:45:76:fc:
                    4a:b0:6c:5c:76:46:91:ff:8d:89:7a:f2:a8:62:73:
                    1d:c4:3a:96:8c:74:82:0a:e9:58:55:73:4a:4e:ec:
                    17:23:43:90:39:69:0e:aa:ac:ec:71:3e:60:e5:6b:
                    0c:e7:7b:f9:8f:93:db:a8:45:ae:d9:43:6f:f1:a1:
                    1c:01:0a:14:33:ce:4f:8c:81:f0:34:b5:cc:7c:81:
                    f6:91:1a:69:31:dc:8a:d1:c1:cc:34:6f:96:71:e0:
                    c2:86:79:37:47:a7:e4:c8:71:3f:44:82:38:7e:11:
                    4d:05:96:fd:01:d8:8c:8b:75:0b:bc:6e:ad:37:1d:
                    77:94:0b:2a:15:1a:43:3c:f6:59:61:eb:ea:8a:73:
                    54:06:b0:ed:70:11:77:42:57:59:e1:80:df:eb:0b:
                    36:d7:7b:d6:c8:53:20:e7:3a:cb:7c:95:67:ea:ff:
                    25:06:80:e9:93:b2:1d:a0:58:9f:ec:60:65:76:e8:
                    24:2c:14:9d:86:47:83:3b:b9:66:59:7d:69:b5:bd:
                    46:af:4f:15:a7:21:45:d1:8c:a1:9b:8b:73:20:94:
                    17:0e:1b:da:d2:e3:93:fb:98:d8:db:13:2b:ed:ff:
                    f5:95
                Exponent: 65537 (0x10001)
        X509v3 extensions:
            X509v3 Authority Key Identifier:
                keyid:8D:FB:23:BD:1D:AA:3B:C0:12:62:2A:15:8F:27:BF:81:EB:94:15:42

            X509v3 Basic Constraints:
                CA:FALSE
            X509v3 Key Usage:
                Digital Signature, Non Repudiation, Key Encipherment, Data Encipherment
            X509v3 Subject Alternative Name:
                DNS:localhost, IP Address:192.168.11.100, IP Address:192.168.10.200, DNS:xa.it.com, DNS:xiykj.com, DNS:*.xa.com
    Signature Algorithm: sha256WithRSAEncryption
         74:0f:a7:56:97:66:e1:8d:7a:5e:4e:7b:6f:b0:da:26:31:5c:
         a3:77:9d:7f:25:19:1c:e2:cd:6a:ee:b3:9e:1f:55:3e:ea:8c:
         05:5b:0f:9e:ac:f7:0f:72:8b:4c:6e:eb:20:4c:c3:d4:7b:d1:
         63:79:54:dc:8d:46:f5:2e:73:fe:4f:5e:8f:d2:3c:54:47:90:
         ae:cd:20:28:31:19:88:26:ec:46:86:1c:1e:ab:8b:67:77:d6:
         81:1d:62:1b:59:7c:d6:4d:52:fe:44:b7:18:ce:6d:47:d3:34:
         48:c8:59:c9:f9:3a:2a:41:9a:7c:50:c0:43:b0:6a:f4:3c:b1:
         d5:49:f1:be:21:ae:b9:d8:72:48:19:f8:20:8c:3b:03:c5:c7:
         26:0d:27:08:4d:0b:9e:27:ea:3c:bf:c3:09:07:fe:b0:68:9c:
         76:1a:3f:49:44:af:67:9f:47:af:88:9f:50:25:1c:f4:a3:05:
         b6:fb:1c:04:16:3d:6d:d3:ac:99:92:73:05:2f:c8:08:9a:e4:
         88:e4:12:4d:f3:d0:aa:47:3c:eb:cf:9b:20:3a:88:e5:33:1b:
         32:65:14:78:1d:c4:24:e8:63:e7:8e:18:b3:2b:bb:e2:94:38:
         1d:dd:1f:f5:13:a2:db:ef:65:bc:12:a9:66:4f:48:15:57:e8:
         82:79:24:a0
```

9. 使用 CA 验证 ssl 证书状态，若显示“OK”则表示通过验证：


```shell
$ openssl verify -CAfile root.crt server.crt
server.crt: OK
```

最后，将 root.crt 导入到需要访问的客户端 PC 的“受信任的根证书颁发机构”中，同时将 server.crt、server.key 文件部署在服务器上即可。
