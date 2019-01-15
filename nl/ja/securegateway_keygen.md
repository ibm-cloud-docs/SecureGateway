---

copyright:
  years: 2015, 2017
lastupdated: "2017-04-25"

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 証明書/鍵の管理

## 自己署名証明書/鍵ペアの生成

自己署名証明書/鍵ペアを生成するには、以下のコマンドを実行します。

```
openssl req -new -newkey rsa:2048 -days 365 -nodes -x509 -keyout serverKey.pem -out serverCert.pem
```
{: pre}


## ブラウザーへの証明書/鍵ペアのアップロード

相互認証を実施する宛先にアクセスするには、証明書/鍵ペアを PKCS#12 ファイルに変換し、それをブラウザーにアップロードする必要があります。

PKCS#12 ファイルを作成するには、以下のコマンドを使用します。

```
openssl pkcs12 -export -in <path_to_cert> -inkey <path_to_key> -name "my-sg-pair" -out <path_to_write_to>.p12
```
{: pre}

Firefox では、「オプション」 > 「設定」 > 「詳細設定」 > 「証明書」 > 「証明書を表示」 > 「あなたの証明書」 > 「インポート」と選択して、.p12 ファイルをアップロードできます。

Chrome では、「設定」 > 「HTTPS/SSL」 > 「証明書の管理」 > 「インポート」と選択して、.p12 ファイルをアップロードできます。

Internet Explorer では、「設定」 > 「コンテンツ」 > 「証明書」 > 「インポート」と選択して、.p12 ファイルをアップロードできます。
