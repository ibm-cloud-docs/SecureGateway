---

copyright:
  years: 2015, 2017
lastupdated: "2017-04-25"

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 憑證/金鑰管理
{: #cert-key-management}

## 產生自簽憑證/金鑰配對
{: #self-signed-cert-gen}

若要產生自簽憑證/金鑰配對，請執行下列指令：

```
openssl req -new -newkey rsa:2048 -days 365 -nodes -x509 -keyout serverKey.pem -out serverCert.pem
```
{: pre}


## 將憑證/金鑰配對上傳至瀏覽器
{: #upload-cert-to-browser}

若要存取強制執行交互鑑別的目的地，您必須將憑證/金鑰配對轉換為 PKCS#12 檔案，並將其上傳至瀏覽器。

若要建立 PKCS#12 檔案，請使用下列指令：

```
openssl pkcs12 -export -in <path_to_cert> -inkey <path_to_key> -name "my-sg-pair" -out <path_to_write_to>.p12
```
{: pre}

在 Firefox 中，.p12 檔案可以上傳至「選項」>「喜好設定」>「進階」>「憑證」>「檢視憑證」>「您的憑證」>「匯入」

在 Chrome 中，.p12 檔案可以上傳至「設定」> HTTPS/SSL >「管理憑證」>「匯入」

在 Internet Explorer 中，.p12 檔案可以上傳至「設定」>「內容」>「憑證」>「匯入」
