---

copyright:
  years: 2015, 2017
lastupdated: "2017-04-25"

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# クライアントのアンインストール

## Docker
{: #docker}

{{site.data.keyword.SecureGateway}} クライアント Docker イメージを削除するには、以下のコマンドを実行します。

```
docker rmi ibmcom/secure-gateway-client
```
{: pre}

## Mac OS X
{: #mac}

Mac OS X から {{site.data.keyword.SecureGateway}} クライアントを削除するには、端末から以下のコマンドを実行します。

```
cd /Applications
rm -rf ./ibm
```
{: codeblock}

## Linux
{: #linux}

### Ubuntu/PowerPC
{: #debian-uninstall}

{{site.data.keyword.SecureGateway}} クライアントのインストール状況を確認するには、以下のコマンドを使用します。

```
sudo dpkg -l | grep ibm-securegateway-client
```
{: pre}

インストール・ディレクトリー、構成ファイル、ID、グループ ID、およびログ・ファイルを削除せずに {{site.data.keyword.SecureGateway}} クライアントをアンインストールするには、以下を使用します。

```
sudo dpkg -r ibm-securegateway-client
```
{: pre}

{{site.data.keyword.SecureGateway}} クライアントをアンインストールし、すべてを削除するには、以下を使用します。

```
sudo dpkg --purge ibm-securegateway-client
```
{: pre}

### RedHat/SuSE
{: #rpm-uninstall}

{{site.data.keyword.SecureGateway}} クライアントのインストール状況を確認するには、以下のコマンドを使用します。

```
rpm -qa | grep ibm-securegateway-client
```
{: pre}

インストール・ディレクトリー、構成ファイル、ID、グループ ID、およびログ・ファイルを削除せずに {{site.data.keyword.SecureGateway}} クライアントをアンインストールするには、以下を使用します。

```
rpm -e ibm-securegateway-client
```
{: pre}

{{site.data.keyword.SecureGateway}} クライアントをアンインストールし、すべてを削除するには、以下を使用します。

```
yum remove ibm-securegateway-client
```
{: pre}

## Windows
{: #windows}

Windows で {{site.data.keyword.SecureGateway}} クライアントのインストールを削除するには、次の 2 つの方法があります。

1. インストール・ディレクトリーから削除を実行する: {{site.data.keyword.SecureGateway}} クライアントのインストール・ディレクトリーに移動し、uninstall.exe をダブルクリックします。

2. コントロール・パネルから削除を実行する: コントロール・パネルの「プログラム」セクションから {{site.data.keyword.SecureGateway}} アプリケーションを削除します。
