---

copyright:
  years: 2015, 2018
lastupdated: "2018-08-10"

---

# クライアントを実行するための要件

## システム要件
{: #system}

Secure Gateway クライアントは、以下の環境でサポートされます。

| 名前 | バージョン           |
| ------------- | ----------- |
| Windows Desktop | 7、8.1、10 以降 |
| Windows Server | 2012 R2 以降 |
| Red Hat Linux | 6.5 以降 |
| CentOS | 7.2 以降 |
| SuSE Linux | 11.0<sup>*</sup> 以降 |
| Ubuntu Linux | 14.04 (LTS) 以降 |
| Power Machine | ppc64el アーキテクチャー |
| Ubuntu Z-Linux | - |
| AIX | 7.1 以降 |
| Mac OS X | 10.10 (Yosemite) 以降 |
| Docker | 1.7.0 以降、すべてのサポートされるオペレーティング・システム |

<sup> * </sup>- SLES 11 のサポートはサービス・パック 4 適用済みの場合のみです。

<b>注:</b> ネイティブのクライアント・インストールに対しては、現在のところ 64 ビット環境のみがサポートされています。

## ネットワーク要件
{: #network}

Secure Gateway クライアントは、アウトバウンド・ポート 443 およびポート 9000 を使用して、npm レジストリーおよび {{site.data.keyword.Bluemix}} 環境に接続します。
- npm インストール用のポート 443 
  - インストール中に、インストーラーは npm レジストリーに接続し、`npm install` を使用して、Secure Gateway クライアントが必要とする依存関係をインストールします。インストールの前に、クライアントがインストールされるマシンが npm レジストリー Web サイトに接続できることを確認してください。デフォルトでは、npm は https://registry.npmjs.org にある npm, Inc. の公開レジストリーを使用するように構成されます。<br><br>
ご使用の環境に npm Enterprise サーバーがある場合、npm Enterprise サーバー上で Secure Gateway クライアントのすべての依存関係をホワイトリストに登録してください。依存関係のリストについては、`<Installation_directory>&#xa5;ibm&#xa5;securegateway&#xa5;client&#xa5;package.json` ファイルを参照してください。<br><br>

- ゲートウェイ認証用のポート 443


  | 地域  | ホスト  |
  | --  | --  |
  | 米国南部  | sgmanager.ng.bluemix.net  |
  | 米国東部  | sgmanager.us-east.bluemix.net  |
  | 英国  | sgmanager.eu-gb.bluemix.net  |
  | ドイツ  | sgmanager.eu-de.bluemix.net  |
  | シドニー  | sgmanager.au-syd.bluemix.net  |


- ポート 9000

  SG ゲートウェイのノード。これはゲートウェイの構成にあります。各ゲートウェイは同じノードにはありません。ゲートウェイを作成するたびにノードのホスト名を確認してください。


必ず、適用される追加のファイアウォールおよび IP テーブルについて、ルールの確認または変更を行ってください。 ネットワーク管理者が特定の IP を必要とする場合は、[お客様の環境に合わせてサポートにそれらを要求](/docs/services/SecureGateway/securegateway_troubleshooting.html#support)してください。


## ハードウェア要件の判別
{: #hardware}

Secure Gateway クライアントを実行するマシンの仕様は、各接続を通るトラフィックに大きく依存します。クライアントの各インスタンスは、最大 250 個の同時接続を提供する個別のプロセスです。  マシンがサポートする必要のある平均最大値を見積もるには、クライアントを通過する (要求と応答の両方の) トランザクションの平均サイズを判別し、同時トランザクション数 250 を考慮してそれを大きくします。この数値が要求サイズと応答サイズの両方を結合したものであることを考えると、クライアントはこのメモリー占有スペースを超えないはずです。より正確な見積もりを行うには、要求サイズと応答サイズの混合を使用して、実際のトランザクション・シナリオをより適切にシミュレートする必要があります。

Secure Gateway クライアントの単一インスタンスを実行する場合、最小 2 個のコアと 4 GB のメモリーが推奨されます。

同じマシン上でクライアントの複数インスタンスを実行する HA シナリオの場合、最小 4 個のコアと 8 GB のメモリーが推奨されます。
