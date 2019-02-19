---

copyright:
  years: 2015, 2017
lastupdated: "2017-04-25"

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 配置用戶端的自動啟動

## Linux
{: #linux}

如果您已選擇使用系統的自動啟動機能，請使用下面其中一種方法來啟動用戶端。可在下列位置找到可供用戶端使用的配置檔：

```
/etc/ibm/sgenvironment.conf
```
{: screen}

<b>附註：</b>自動啟動功能不適用於 Red Hat Linux 6。

此檔案包含下列要設定的重要變數：

|環境變數|說明|
| ------------- | ----------- |
|RESTART_CLIENT|在安裝或升級期間停止並重新啟動用戶端（Yes 或 No）|
|GATEWAY_ID|在 Bluemix 使用者介面的 Secure Gateway 上所建立的閘道 ID|
|SECTOKEN|此「閘道 ID」的安全記號（如果您選擇在建立閘道時強制執行安全的話）|
|ACL_FILE|您要用來限制對資源之內部部署存取的「存取控制清單檔案」|
|LOGLEVEL|您要對服務設定的記載層次（預設值為 INFO）|
|USE_UI|如果您不要啟動用戶端使用者介面，請將此設為 'N'|
|UI_PORT|您要在其上啟動用戶端使用者介面的埠（預設值為 9003）|
|LANGUAGE|您要讓用戶端日誌使用的語言（預設值是 en）|

<b>附註：</b>只有在您使用系統的自動啟動機能時，才會讀取此檔案。如果您手動執行用戶端，則會忽略此檔案。

### Upstart
{: #upstart}

### 啟動用戶端
{: #upstart-start}

如果您使用 Upstart 功能，以在系統啟動時自動執行 Secure Gateway 用戶端，則必須先檢查其配置 (/etc/ibm/sgenvironment.conf)。在您配置 Upstart 服務之後，即可使用下列指令予以啟動：

```
sudo initctl start securegateway_client
```
{: pre}

若要重新啟動服務，請執行下列指令：

```
sudo initctl restart securegateway_client
```
{: pre}

### 停止用戶端
{: #upstart-stop}

若要停止服務，請執行下列指令：

```
sudo initctl stop securegateway_client
```
{: pre}

### SystemD
{: #systemd}


### 啟動用戶端
{: #systemd-start}

如果您使用 systemD 功能，以在系統啟動時自動執行 Secure Gateway 用戶端，則可能需要先檢查其配置 (/etc/ibm/sgenvironment.conf)。在您配置 Upstart 服務之後，即可使用下列指令予以啟動：

```
systemctl start securegateway_client
```
{: pre}

若要重新啟動服務，請執行下列指令：

```
systemctl restart securegateway_client
```
{: pre}

如需服務上的狀態：

```
systemctl status securegateway_client
```
{: pre}

### 停止用戶端
{: #systemd-stop}

若要停止服務，請執行下列指令：

```
systemctl stop securegateway_client
```
{: pre}

### System V
{: #systemv}

在安裝期間，不會像其他自動啟動機能一樣設定 System V。安裝目錄 /opt/ibm/securegateway/client/upstart 中提供了 Script，包括：

```
secgw.sh
securegateway_clientd
```
{: screen}

<b>附註：</b>本節會影響 SuSE/SLES 11 使用者。

選用項目：如果您執行的 SuSE 第 11 版使用 System V 自動啟動，則提供了配置此處理程序用的 Script。請遵循以下程序：

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

執行這些步驟之後，可以使用 YasT 及 System V 指令來啟動/停止常駐程式。

回到[開始使用 - 新增用戶端](/docs/services/SecureGateway/securegateway_client.html)。

## Windows
{: #windows}

若要變更 Windows 服務的狀態，請使用管理者專用權開啟指令視窗。

```
cd "<Installation_directory>\ibm\securegateway\client"
```
{: pre}

如果服務已在執行中，則使用者可以使用以下指令予以停止：

```
windowsService.cmd uninstall
```
{: pre}

若要啟動服務，請使用下列指令：

```
windowsService.cmd install
```
{: pre}

<b>附註：</b>將根據下列位置中儲存的配置來執行服務：

```
%Installation_directory%/ibm/securegateway/client/securegw_service.config
```
{: pre}

Windows 服務的應用程式日誌將儲存在：

```
%Installation_directory%/ibm/securegateway/client/logs/securegw_win_service.log
```
{:pre}

 日誌會每日合成為一個新的檔案。

回到[開始使用 - 新增用戶端](/docs/services/SecureGateway/securegateway_client.html)。
