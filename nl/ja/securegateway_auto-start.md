---

copyright:
  years: 2015, 2017
lastupdated: "2017-04-25"

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# クライアントの自動開始の構成
{: #auto-start-conf}

## Linux
{: #linux}

システムの自動開始機能を使用することを選択した場合、以下のいずれかの方法を使用してクライアントを開始します。クライアントによって使用される構成ファイルは、以下の場所にあります。

```
/etc/ibm/sgenvironment.conf
```
{: screen}

<b>注:</b> Red Hat Linux 6 では、自動開始機能は利用できません。

このファイルに含まれている以下の重要な変数を設定する必要があります。

| 環境変数 | 説明       |
| ------------- | ----------- |
| RESTART_CLIENT | インストール中またはアップグレード中にクライアントの停止と再始動を行います (Yes または No) |
| GATEWAY_ID | Secure Gateway for Bluemix UI で作成されたゲートウェイ ID |
| SECTOKEN | このゲートウェイ ID 用のセキュリティー・トークン (作成時にセキュリティーを施行することを選択した場合) |
| ACL_FILE | リソースへのオンプレミス・アクセスを制限するために使用するアクセス制御リスト・ファイル |
| LOGLEVEL | サービスに設定するログ・レベル (デフォルトは INFO) |
| USE_UI   | クライアント UI を起動しない場合はこれを「N」に設定します |
| UI_PORT  | クライアント UI を起動するポート (デフォルトは 9003) |
| LANGUAGE | クライアントのログインに使用する言語 (デフォルトは en) |

<b>注:</b> このファイルが読み取られるのは、システムの自動開始機能を使用する場合のみです。  クライアントを手動で実行する場合、このファイルは無視されます。

### Upstart
{: #upstart}

### クライアントの開始
{: #upstart-start}

システム始動時に Secure Gateway クライアントが自動的に実行されるように upstart 機能を使用する場合、最初に構成 (/etc/ibm/sgenvironment.conf) を検査する必要があります。upstart サービスの構成が終わったら、以下のコマンドを使用して開始できます。

```
sudo initctl start securegateway_client
```
{: pre}

サービスを再始動するには、次のようにします。

```
sudo initctl restart securegateway_client
```
{: pre}

### クライアントの停止
{: #upstart-stop}

サービスを停止するには、次のコマンドを実行します。

```
sudo initctl stop securegateway_client
```
{: pre}

### SystemD
{: #systemd}


### クライアントの開始
{: #systemd-start}

システム始動時に Secure Gateway クライアントが自動的に実行されるように systemD 機能を使用する場合、最初に構成 (/etc/ibm/sgenvironment.conf) を検査する必要があります。upstart サービスの構成が終わったら、以下のコマンドを使用して開始できます。

```
systemctl start securegateway_client
```
{: pre}

サービスを再始動するには、次のようにします。

```
systemctl restart securegateway_client
```
{: pre}

サービスの状況を確認するには、次のようにします。

```
systemctl status securegateway_client
```
{: pre}

### クライアントの停止
{: #systemd-stop}

サービスを停止するには、次のコマンドを実行します。

```
systemctl stop securegateway_client
```
{: pre}

### SystemV
{: #systemv}

System V は、他の自動開始機能とは異なり、インストール時にはセットアップされません。インストール・ディレクトリー /opt/ibm/securegateway/client/upstart にある以下のスクリプトが使用可能です。

```
secgw.sh
securegateway_clientd
```
{: screen}

<b>注:</b> このセクションは SuSE/SLES 11 のユーザーに影響します。

オプション: systemV 自動開始を使用する SuSE バージョン 11 を実行する場合、提供されているスクリプトを使用してこのプロセスを構成できます。以下の手順を実行してください。

```
cd /opt/ibm/securegateway/client/upstart/systemV
cp secgw.sh /usr/local/bin
chmod +x /usr/local/bin/secgw.sh
cp securegateway_clientd /usr/local/bin
chmod +x /usr/local/bin/securegateway_clientd
cd /etc/init.d
ln -s /usr/local/bin/securegateway_clientd
insserv securegateway_clientd
vi /etc/ibm/sgenvironment.conf
```
{: codeblock}

これらのステップが実行された後、YasT および systemV コマンドを使用してデーモンを開始/停止できます。

[概説 - クライアントの追加](/docs/services/SecureGateway/securegateway_client.html)に戻ります。

## Windows
{: #windows}

Windows サービスの状態を変更するには、管理者特権でコマンド・ウィンドウを開きます。

```
cd "<Installation_directory>\ibm\securegateway\client"
```
{: pre}

サービスが既に実行中の場合、ユーザーは以下のコマンドを使用してサービスを停止できます。

```
windowsService.cmd uninstall
```
{: pre}

サービスを開始するには、次のコマンドを使用します。

```
windowsService.cmd install
```
{: pre}

<b>注:</b> サービスは以下に保管されている構成に基づいて実行されます。

```
%Installation_directory%/ibm/securegateway/client/securegw_service.config
```
{: pre}

Windows サービスのアプリケーション・ログは次の場所に保管されます。

```
%Installation_directory%/ibm/securegateway/client/logs/securegw_win_service.log
```
{:pre}

 ログは日次ベースで新しいファイルに切り替えられます。

[概説 - クライアントの追加](/docs/services/SecureGateway/securegateway_client.html)に戻ります。
