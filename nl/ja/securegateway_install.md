---

copyright:
  years: 2015, 2019
lastupdated: "2019-05-07"

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# クライアントのインストール
{: #client-install}

## Docker
{: #installing-docker}

Docker は、ほとんどあるいはまったく構成を必要とせずに、アプリケーションを素早く簡単にインストールするためのコンテナー・アプローチを提供するサード・パーティーのプラットフォームです。 {{site.data.keyword.SecureGateway}} サービスは、Docker ユーティリティーがワークステーションにインストールされた後に使用される Docker イメージを提供します。  Docker をインストールするには、Docker のインストールについての Web サイトを参照し、ご使用のシステム向けの指示に従ってください。

### クライアントのインストールおよび更新
{: #docker-install}

{{site.data.keyword.SecureGateway}} クライアント Docker イメージのインストールおよび更新は両方とも同じコマンドを使用します。

```
docker pull ibmcom/secure-gateway-client
```
{: pre}

### 対話式クライアント・セッションの開始
{: #docker-run}

IBM {{site.data.keyword.SecureGateway}} の Docker コンテナーを実行するには、以下のコマンドを実行します。

```
docker run -it ibmcom/secure-gateway-client <gateway ID> -t <security token>
```
{: pre}

通常、これには 2 つのパラメーターがあります。1 つは {{site.data.keyword.SecureGateway}} ゲートウェイ ID、もう 1 つはゲートウェイのセキュリティー・トークンであり、両方とも {{site.data.keyword.SecureGateway}} ダッシュボードを介して入手できます。

### サポートされる Docker コマンド
{: #docker-commands}

{{site.data.keyword.SecureGateway}} クライアントは、コンテナーを操作するために `pull` コマンドおよび `run` コマンドのみをサポートします。

[概説 - クライアントの追加](/docs/services/SecureGateway?topic=securegateway-add-client)に戻ります。

## Mac OS X
{: #installing-mac}

### Mac OS X で実行するための要件
{: #mac-requirements}

Mac OS X でクライアントを実行するには、以下の前提条件が満たされる必要があります。
 1. Xcode 開発ツールをインストールします。これは、このプラットフォーム上の一部のノード・モジュールで必要です。

### Mac OS X インストール手順
{: #mac-install}

ご使用のシステムのセキュリティー・セットアップによっては、このインストールを実行するために管理特権が必要な場合があります。

 1. {{site.data.keyword.SecureGateway}} UI でダウンロードされた DMG イメージを、通常はダブルクリックすることによってマウントします。
 2. 新しい「ファインダー」ウィンドウが表示されます。 表示されない場合は、マウントされたボリュームをダブルクリックしてください。 このウィンドウには、「ibm」フォルダー・アイコンとアプリケーションのショートカット・アイコンが含まれているので、「ibm」フォルダーをショートカットにドラッグ・アンド・ドロップします。

### 対話式クライアント・セッションの開始
{: #mac-run}

クライアントを開始するには、デフォルトのインストール場所 `/Applications/ibm/` にある `secgw.command` ファイルを実行します。

[概説 - クライアントの追加](/docs/services/SecureGateway?topic=securegateway-add-client)に戻ります。

## Linux
{: #installing-linux}

インストールには、{{site.data.keyword.SecureGatewayfull}} クライアントと、セキュアなバージョンの IBM の nodejs パッケージが含まれます。  両方ともシステムの /opt/ibm ディレクトリーにインストールされます。  インストーラーは、以下を作成または更新します。

| ディレクトリー | ファイル名 | 説明          |
| ------------- | ------------- | ----------- |
| /etc/ibm | sgenvironment.conf | upstart 用構成ファイル |
| /etc/ibm | passwd | 次のユーザーを作成します: secgwadmin:x:501:501::/home/secgwadmin:/bin/bash |
| /etc/ibm | group | 次のグループを作成します: secgwadmin:x:501: |
| /etc/init | securegateway_client.conf | upstart ファイル |
| /opt/ibm | node-v&lt;version&gt;-linux-x64 | IBM の Node JS インストール |
| /opt/ibm | securegateway | {{site.data.keyword.SecureGateway}} クライアントのインストール済み環境 |
| /usr/local/bin | node | IBM のノード実行可能ファイル用に作成された symlink |
| /usr/local/bin | npm | IBM の npm 実行可能ファイル用に作成された symlink |
| /var/log/securegateway | client_console.log | 自動開始プロセスとしてクライアントを実行中に作成されるログ・ファイル |

このインストールを実行するには、root 特権または管理特権が必要です。

### Ubuntu/PowerPC インストール済み環境
{: #debian-install}

 1. {{site.data.keyword.SecureGateway}} クライアントをインストールします。 例えば、Debian パッケージを使用する場合は、以下のコマンドを実行します。

    ```
    sudo dpkg -i <secure-gateway-debian-package>
    ```
    {: pre}

 2. クライアント・インストーラーが開始すると、以下の情報を求めるプロンプトが出ます。

    <b>自動開始プロセス Yes/No</b>
        これがアップグレードまたは再インストールである場合、インストール・プロセスの実行中は既存のクライアントを停止しておくかどうかについて選択する必要があります。 既存のクライアントを停止するには Y/y を選択します。 そうでない場合、クライアント・パッケージはアップグレードされますが、クライアントは再始動されません。つまり、適切なソフトウェア更新機会が来て再始動が実行されるのを待たなければならない可能性があります。 N/n を選択すると、インストールが続行され、手動でクライアントを再始動する必要があります。

    <b>ゲートウェイ ID</b>
        クライアントのゲートウェイ ID を設定します。 ゲートウェイ ID は、{{site.data.keyword.SecureGateway}} サービスの作成時に定義されます。 クライアントが接続に失敗する場合は、.conf ファイルを編集して選択内容を変更できます。

    <b>セキュリティー・トークン</b>
        ゲートウェイ ID でセキュリティー・トークンの検査が有効になっている場合は、ここでセキュリティー・トークンを指定する必要があります。 ゲートウェイ ID にセキュリティー・トークンが必要なければ、ここはブランクのままにしておいてください。

    <b>ロギング・レベル</b>
        デフォルト設定は INFO です。 有効な値は他に、TRACE、DEBUG、および ERROR があります。

    <b>アクセス制御リスト</b>
        オンプレミス・リソースへのアクセス権に関するアクセス制御リストを含んでいるファイル名の絶対パスを入力します。 詳しくは、/opt/ibm/securegateway にある README.md ファイルまたはアクセス制御リストを参照してください。

    <b>クライアント UI</b>
        クライアント UI を使用するかどうかを選択します。 使用する場合、ユーザーは UI 用のポートを変更できます。 デフォルト値は 9003 です。

    <b>注:</b> どのプロンプトも応答は必須ではありません。定義されているデフォルトが取り込まれるか、sgenvironment.conf ファイル内でブランクのままになります。 これにより、インストール・プロセスをユーザーと対話せずに実行できるようになります。

    <b>注:</b> システムの upstart プロセスを使用してクライアントを開始または再始動するたびに、sgenvironment.conf が読み取られます。 いつでも /etc/ibm/sgenvironment.conf ファイルを編集して構成を変更でき、クライアントを再始動してその変更内容を取り込むことができます。

    <b>注:</b> {{site.data.keyword.SecureGateway}} クライアントのサービス・ログの言語を変更するには、/etc/ibm/sgenvironment.conf ファイルの `LANGUAGE` パラメーターを変更します。 サービスを再始動すると、サービス・ログは選択した言語に変更されます。

 3. クライアントを再始動した場合、クライアントが正しく実行されていることを確認するには、以下のコマンドを実行します。

    ```
    cat /var/log/securegateway/client_console.log
    ```
    {: pre}

 4. 構成ファイルを編集する場合、構成ファイルが正しく更新されたことを確認するには、以下のコマンドを実行します。

    ```
    sudo vi /etc/ibm/sgenvironment.conf
    ```
    {: pre}

 5. クライアントのインストール状況を確認するには、以下のコマンドを実行します。

    ```
    sudo dpkg --status ibm-securegateway-client
    ```
    {: pre}

返される出力の例を以下に示します。

```
Package: ibm-securegateway-client
Status: install ok installed
Priority: extra
Section: IBM/net
Maintainer: cloudoe-dev@webconf.ibm.com
Architecture: amd64
Source: ibm-securegateway+securegateway-client
Version: 1.7.0
Description: IBM Secure Gateway Client for Bluemix
IBM Secure Gateway client package for Bluemix, supports secure connections to on-premises connections.
Homepage: https://ng.bluemix.net/
License: http://www.ibm.com/software/sla/sladb.nsf/lilookup/986C7686F22D4D3585257E13004EA6CB?OpenDocument
```
{: screen}

### RedHat/SuSE インストール済み環境
{: #rpm-install}

1. {{site.data.keyword.SecureGateway}} クライアントをインストールします。 例えば、RPM パッケージを使用する場合は、以下のコマンドを実行します。

   ```
   rpm -ivhf <secure-gateway-rpm-package>
   ```
   {: pre}


   クライアント・インストーラーが開始し、クライアントがインストールされると、/etc/ibm に sgenvironment.conf ファイルが作成されます。

2. オプション: システムの upstart プロセスを使用する場合、クライアントが正しく開始するためには、このファイルを編集して、以下を指定する必要があります。 この構成ファイルの編集について詳しくは、[Upstart の使用](/docs/services/SecureGateway?topic=securegateway-auto-start-conf#auto-start-linux)を参照してください。

3. upstart を使用してクライアントを開始した場合、ログ・ファイルを調べて、クライアントが正しく実行されていることを確認します。

   ```
   cat /var/log/securegateway/client_console.log
   ```
   {: pre}

4. クライアントのインストール状況を確認するには、以下のコマンドを実行します。

   ```
   rpm -q ibm-securegateway-client
   ```
   {: pre}

### AIX インストール済み環境
{: #aix-install}

1. パッケージに対する実行権限が設定されていることを確認します。 必要な場合、以下のコマンドを発行してファイル許可を変更します。
    ```
    chmod a+x <secure-gateway-bin-package>
    ```
2. 以下のコマンドを発行して、パッケージを取り出します。
    ```
    ./<secure-gateway-bin-package>
    ```

注:
ご使用の AIX システムが、インストールされる Node.js および ksh を実行するための要件を満たしていることを確認してください。

### 対話式クライアント・セッションの開始
{: #linux-run}

クライアントを開始するには、以下のコマンドを実行します。

```
cd /opt/ibm/securegateway/client
node lib/secgwclient.js <gateway ID> -t <security token>
```
{: codeblock}

通常、これには 2 つのパラメーターがあります。1 つは {{site.data.keyword.SecureGateway}} ゲートウェイ ID、もう 1 つはゲートウェイのセキュリティー・トークンであり、両方とも {{site.data.keyword.SecureGateway}} ダッシュボードを介して入手できます。

[概説 - クライアントの追加](/docs/services/SecureGateway?topic=securegateway-add-client)に戻ります。

## Windows
{: #installing-windows}

### クライアントのインストール
{: #windows-install}

ご使用のシステムのセキュリティー・セットアップによっては、このインストールを実行するために管理特権が必要な場合があります。

 1. EXE インストーラー・ファイルをシステムにコピーします。  インストール・パッケージの整合性のチェックに使用できる MD5 サムも提供されます。

 2. それをダブルクリックしてプロンプトに応答するか、コマンド・プロンプトから実行可能ファイルを実行してインストールします。

 3. ユーザーにプロンプトが出され、インストール・ディレクトリーを選択するよう求められます。 デフォルトの場所は、Program Files (x86) ディレクトリーです。

 4. ユーザーにプロンプトが出され、コマンド・ライン・インターフェースを起動する際の言語を選択するよう求められます。 言語が選択されない場合、デフォルトで英語に設定されます。

 5. ユーザーにプロンプトが出され、クライアントを Windows サービスとしてインストールするように求められます。 ユーザーがそうすることを選択すると、クライアントは、後続のダイアログでユーザーが指定する構成を使用して、Windows サービスとしてバックグラウンドで実行されます。 サービスの状況は、「コントロール パネル」のサービス・ページで確認できます。 サービス名は「IBM Bluemix Secure Gateway Service」です。

 6. インストーラーは、マシン上に既存のインストール済み環境があるかどうかを識別します。 検出された場合、ユーザーは既存の構成を使用するかどうかを尋ねられます。検出されない場合、ユーザーは新しい構成を入力することができます。 各ゲートウェイのゲートウェイ ID、セキュリティー・トークン、ACL ファイル、ログ・レベルなどの詳細をここで指定できます。 ユーザーがクライアントを Windows サービスとして実行することを選択した場合、クライアントは指定された構成を使用して起動されます。 ユーザーがクライアントを Windows サービスとして起動することを選択しなかった場合、構成は将来の使用のために保管されます。 いずれの場合も、構成は %Installation_directory%/ibm/securegateway/client/securegw_service.config に保管されます。

 7. バージョン 1.5.0 以降、クライアントには完全な機能を備えたクライアント UI が付属していて、インストーラー自体からそれを構成できます。 ユーザーは、パスワードと、クライアント UI の起動元にするポート番号 (デフォルトは 9003) を指定できます。

 ユーザーが複数のゲートウェイに接続してクライアントを起動することを選択した場合は、構成値を指定する際に注意してください。 ゲートウェイ ID はスペースで区切る必要があります。 セキュリティー・トークン、ACL ファイル、およびログ・レベルは、-- で区切る必要があります。 特定のゲートウェイに対してこれら 3 つの値を指定しない場合、「none」を使用してください。

 <b>注:</b> 空白文字が残っていないように注意してください。

 <b>注:</b> ACL ファイルは %Installation_directory%/ibm/securegateway/client ディレクトリーまたはそのパスに相対的に配置する必要があることに注意してください。

### 対話式クライアント・セッションの開始
{: #windows-run}

クライアントを開始するには、管理特権でコマンド・プロンプトを開き、以下のコマンドを実行します。

```
cd "<Installation_directory>\ibm\securegateway\client"
secgw.cmd
```
{: codeblock}

あるいは、`<Installation_directory>&#xa5;ibm&#xa5;securegateway&#xa5;client` にナビゲートし、`secgw.cmd` をダブルクリックします。

<b>注:</b> `<Installation_directory>&#xa5;ibm&#xa5;securegateway&#xa5;client&#xa5;securegw_service.config` ファイルに保管された構成を使用することも、詳細を対話式に指定することもできます。

[概説 - クライアントの追加](/docs/services/SecureGateway?topic=securegateway-add-client)に戻ります。

## DataPower
{: #installing-datapower}

DataPower には、組み込みバージョンの {{site.data.keyword.SecureGateway}} クライアントがあります。  {{site.data.keyword.SecureGateway}} クライアントのバージョンは DataPower のバージョンによって異なることがあります。  該当する [DataPower クライアントの制限事項](/docs/services/SecureGateway?topic=securegateway-client-interacting#limits-datapower)に注意してください。古い Secure Gateway クライアントを使用すると、予期しないエラーが発生する可能性があります。

| DataPower バージョン | {{site.data.keyword.SecureGateway}} クライアントのバージョン  |
| -- | --  |
| 7.2.0.0, 7.5.0.0 | 1.1.0  |
| 7.5.1.0, 7.7.0 | 1.4.2  |
| 7.5.2.4 | 1.6.1  |
| 7.5.2.6, 7.6.0.0 | 1.7.0  |
| 7.5.2.14, 7.6.0.7, 7.7.1.0, 2018.4.1.0 |  1.8.0fp6  |
| 2018.4.1.4 | 1.8.2  |
<!-- | 7.6.0.15, 2018.4.1.6 | 1.8.2fp1 | -->

### クライアント・セッションの開始
{: #datapower-run}

1. DataPower Web GUI にログインします。
2. 「Objects」 -> 「Cloud」 -> 「Secure Gateway Client」とナビゲートするか、または、Secure Gateway Client を検索します。
3. 新規クライアント接続を構成するため、「`Add`」をクリックします。
4. 名前、ゲートウェイ ID、およびセキュリティー・トークン (該当する場合) を指定し、変更を適用します。

[概説 - クライアントの追加](/docs/services/SecureGateway?topic=securegateway-add-client)に戻ります。
