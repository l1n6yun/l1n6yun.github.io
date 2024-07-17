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

具体步骤如下：

**操作方法**：

1. 首先创建一个专门用于保存 ca 证书文件的文件夹“ca”：

```plaintext
sudo mkdir ca
cd ca
```

2. 接着创建 ca 私钥（强烈建议设置密码以增强安全性）：


```plaintext
sudo openssl genrsa -des3 -out CA.key 2048
```

3. 生成 ca 证书，自签 20 年有效期，并将此 ca 证书导入需要访问的 PC 的“受信任的根证书颁发机构”中，后续用此 ca 签署的证书均可正常使用：


```plaintext
sudo openssl req -x509 -new -nodes -key CA.key -sha256 -days 7300 -out CA.crt
```

4. 完成上述步骤后，创建 ssl 证书私钥：


```plaintext
cd..
sudo mkdir certs
cd certs/
sudo openssl genrsa -out zabbix.key 2048        #创建 ssl 私钥
```

5. 随后创建 ssl 证书 csr：


```plaintext
sudo  openssl req -new -key zabbix.key -out zabbix.csr        #创建 ssl 证书 csr
```

6. 创建域名附加配置信息，新建一个文件，通过 vim cert.ext ，将以下代码粘贴后保存：


```plaintext
authorityKeyIdentifier=keyid,issuer
basicConstraints=CA:FALSE
keyUsage = digitalSignature, nonRepudiation, keyEncipherment, dataEncipherment
subjectAltName = @alt_names

[alt_names]
DNS.1 = localhost
IP.2 = 192.168.11.100
IP.3 = 192.168.10.200
DNS.4 = xa.it.com
DNS.5 = xiykj.com
DNS.6 = *.xa.com
```

需要注意的是，IP.2 = 192.168.11.100 表示 https 要访问的 IP 地址，IP.3 同理也是 IP 地址，ssl 证书允许自签多个 IP 地址，这便是自签 IP 的证书。而 DNS.4 = [xa.it.com](https://xa.it.com/) 则表示 https 要访问的域名，DNS.5、DNS.6 同理均为域名，ssl 证书也支持自签多个域名，此为自签域名的证书。

7. 查看文件，通过执行“ls -al”命令：


```plaintext
文件列表：

cert.ext          #ssl 证书附加配置信息
serial            #证书序列号
zabbix.crt        #ssl 证书文件，包含公钥信息
zabbix.csr        #ssl 证书签名文件
zabbix.key        #ssl 证书私钥
```

8. 查看签署的证书信息：


```plaintext
sudo openssl x509 -in zabbix.crt -noout -text
```

9. 使用 CA 验证 ssl 证书状态，若显示“OK”则表示通过验证：


```plaintext
sudo openssl verify -CAfile../ca/CA.crt zabbix.crt
```

最后，将 CA.crt 导入到需要访问的客户端 PC 的“受信任的根证书颁发机构”中，同时将 zabbix.crt、zabbix.key 文件部署在服务器上即可。