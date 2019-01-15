---

copyright:
  years: 2015, 2017
lastupdated: "2017-04-25"

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 证书/密钥管理

## 生成自签名证书/密钥对

要生成自签名证书/密钥对，请运行以下命令：

```
openssl req -new -newkey rsa:2048 -days 365 -nodes -x509 -keyout serverKey.pem -out serverCert.pem
```
{: pre}


## 将证书/密钥对上传到浏览器

要访问强制实施相互认证的目标，您必须将证书/密钥对转换为 PKCS#12 文件，并将其上传到浏览器。

要创建 PKCS#12 文件，请使用以下命令：

```
openssl pkcs12 -export -in <path_to_cert> -inkey <path_to_key> -name "my-sg-pair" -out <path_to_write_to>.p12
```
{: pre}

在 Firefox 中，可以通过“选项”>“首选项”>“高级”>“证书”>“查看证书”>“您的证书”>“导入”来上传 .p12 文件

在 Chrome 中，可以通过“设置”>“HTTPS/SSL”>“管理证书”>“导入”来上传 .p12 文件

在 Internet Explorer 中，可以通过“设置”>“内容”>“证书”>“导入”来上传 .p12 文件
