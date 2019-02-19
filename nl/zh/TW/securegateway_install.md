---

copyright:
  years: 2015, 2018
lastupdated: "2018-10-11"

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 安裝用戶端

## Docker
{: #docker}

Docker
是協力廠商平台，它提供容器方式來輕鬆快速地安裝應用程式，且只需要很少配置或不需任何配置。
{{site.data.keyword.SecureGateway}} 服務提供 Docker 映像檔，以便在 Docker 公用程式安裝在工作站之後使用此映像檔。若要安裝 Docker，請參閱 Docker 安裝網站，並遵循您系統的指示。

### 安裝及更新用戶端
{: #docker-install}

安裝及更新「{{site.data.keyword.SecureGateway}} 用戶端」Docker 映像檔都使用相同的指令：

```
docker pull ibmcom/secure-gateway-client
```
{: pre}

### 啟動互動式用戶端階段作業
{: #docker-run}

若要執行 IBM {{site.data.keyword.SecureGateway}} 的 Docker 容器，請發出下列指令：

```
docker run -it ibmcom/secure-gateway-client <gateway ID> -t <security token>
```
{: pre}

通常，它會接受兩個參數：您的 {{site.data.keyword.SecureGateway}} 閘道 ID 及閘道的安全記號，這兩者都可以透過「{{site.data.keyword.SecureGateway}} 儀表板」取得。

### 支援的 Docker 指令
{: #docker-commands}

「{{site.data.keyword.SecureGateway}} 用戶端」只支援用於操作容器的 `pull` 及 `run` 指令。

回到[開始使用 - 新增用戶端](/docs/services/SecureGateway/securegateway_client.html)。

## Mac OS X
{: #mac}

### Mac OS X 的執行需求
{: #mac-requirements}

在 Mac OS X 上執行用戶端之前，需要具備下列必要條件。
 1. 安裝 Xcode 開發工具，此平台上的部分 node 模組需要此工具。

### Mac OS X 安裝程序
{: #mac-install}

視系統的安全設定而定，您可能需要有管理專用權才能執行此安裝。

 1. 裝載從 {{site.data.keyword.SecureGateway}} 使用者介面下載的 DMG 映像檔，方法通常是「按兩下」該映像檔。
 2. 應該會出現新的「搜尋器」視窗。此視窗應該包含一個應用程式「捷徑」圖示，請將應用程式拖放至該捷徑。否則，請「按兩下」已裝載的磁區，然後將應用程式圖示拖放至「搜尋器」資訊看板中的「應用程式」圖示。

### 啟動互動式用戶端階段作業
{: #mac-run}

若要啟動用戶端，請執行位於預設安裝位置 `/Applications/ibm/` 的 `secgw.command` 檔案。

回到[開始使用 - 新增用戶端](/docs/services/SecureGateway/securegateway_client.html)。

## Linux
{: #linux}

安裝包括 {{site.data.keyword.SecureGatewayfull}} 用戶端，以及 IBM nodejs 套件的安全版本。兩者都安裝在系統的 /opt/ibm 目錄下。安裝程式將會建立或更新下列檔案：

|目錄|檔名|說明|
| ------------- | ------------- | ----------- |
| /etc/ibm | sgenvironment.conf | Upstart 的配置檔 |
| /etc/ibm | passwd | 建立使用者：secgwadmin:x:501:501::/home/secgwadmin:/bin/bash |
| /etc/ibm | group | 建立群組：secgwadmin:x:501: |
| /etc/init | securegateway_client.conf | Upstart 檔案 |
| /opt/ibm | node-v&lt;version&gt;-linux-x64 | IBM 的 Node JS 安裝 |
| /opt/ibm | securegateway | {{site.data.keyword.SecureGateway}} 用戶端安裝 |
| /usr/local/bin | node | 針對 IBM node 執行檔所建立的符號鏈結 |
| /usr/local/bin | npm | 針對 IBM npm 執行檔所建立的符號鏈結 |
| /var/log/securegateway | client_console.log | 將用戶端執行為自動啟動處理程序時所建立的日誌檔 |

您需要有 root 或管理專用權，才能執行此安裝。

### Ubuntu/PowerPC 安裝
{: #debian-install}

 1. 安裝 {{site.data.keyword.SecureGateway}} 用戶端。例如，如果您使用 Debian 套件，請發出下列指令。

    ```
    sudo dpkg -i <secure-gateway-debian-package>
    ```
    {: pre}

 2. 用戶端安裝程式啟動時，系統會提示您輸入下列資訊。

    <b>自動啟動處理程序 是/否</b>
        如果這是升級或重新安裝，您必須選擇是否要在安裝處理程序執行時停止現有用戶端。選擇 Y/y，以停止現有用戶端。否則，會在未重新啟動用戶端的情況下升級用戶端套件，這表示您可以等待適當的「軟體更新」時間範圍才執行重新啟動。如果您選擇 N/n，則安裝會繼續，而且您必須手動重新啟動用戶端。

    <b>閘道 ID</b>
        設定用戶端的閘道 ID。當您建立 {{site.data.keyword.SecureGateway}} 服務時會定義閘道 ID。如果用戶端無法進行連接，您可以編輯 .conf 檔案來變更選擇的項目。

    <b>安全記號</b>
        如果啟用閘道 ID 來檢查安全記號，您必須現在就提供它。如果閘道 ID 不需要安全記號，請將此項目保留為空白。

    <b>記載層次</b>
        預設設定值是 INFO。其他有效值包含 TRACE、DEBUG 及 ERROR。

    <b>存取控制清單</b>
        輸入檔名的絕對路徑，該檔案中包含能存取內部部署資源之項目上的存取控制清單。如需相關資訊，請參閱 /opt/ibm/securegateway 中的 README.md 檔案或「存取控制清單」。

    <b>用戶端使用者介面</b>
        選擇是否要使用用戶端使用者介面。如果是，使用者可以變更使用者介面的埠。預設值為 9003。

    <b>附註：</b>您不需要回答任何提示，全都會採用已定義的預設值，或在 sgenvironment.conf 檔案中保留空白。這容許在不需要使用者互動的情況下執行安裝程序。

    <b>附註：</b>每次使用系統的 Upstart 處理程序啟動或重新啟動用戶端時，都會讀取 sgenvironment.conf。您隨時都可以編輯 /etc/ibm/sgenvironment.conf 檔案來變更配置，並重新啟動用戶端來讓那些變更生效。

    <b>附註：</b>您可以藉由變更 /etc/ibm/sgenvironment.conf 檔案中的 `LANGUAGE` 參數，來變更 {{site.data.keyword.SecureGateway}} 用戶端服務日誌的語言。服務重新啟動之後，服務日誌將變更為選取的語言。

 3. 如果您已重新啟動用戶端，若要確定用戶端正在適當地執行，請發出下列指令：

    ```
    cat /var/log/securegateway/client_console.log
    ```
    {: pre}

 4. 如果您要編輯配置檔，若要確定已正確地更新配置檔，請發出下列指令：

    ```
    sudo vi /etc/ibm/sgenvironment.conf
    ```
    {: pre}

 5. 若要檢查用戶端安裝的狀態，請發出下列指令：

    ```
    sudo dpkg --status ibm-securegateway-client
    ```
    {: pre}

即會傳回輸出的範例：

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

### RedHat/SuSE 安裝
{: #rpm-install}

1. 安裝 {{site.data.keyword.SecureGateway}} 用戶端。例如，如果您使用 RPM 套件，請發出下列指令：

   ```
   rpm -ivhf <secure-gateway-rpm-package>
   ```
   {: pre}


   用戶端安裝程式會啟動並安裝用戶端，並在 /etc/ibm 中建立 sgenvironment.conf 檔案。

2. 選用項目：如果您要使用系統的 Upstart 處理程序，則必須編輯此檔案並提供下列內容，用戶端才能正確啟動。如需編輯此配置檔的相關資訊，請參閱[使用 Upstart](/docs/services/SecureGateway/securegateway_auto-start.html#linux)。

3. 如果您已使用 Upstart 來啟動用戶端，請檢查日誌檔，確定其正確執行中。

   ```
   cat /var/log/securegateway/client_console.log
   ```
   {: pre}

4. 若要檢查用戶端安裝的狀態，請發出下列指令：

   ```
   rpm -q ibm-securegateway-client
   ```
   {: pre}

### 啟動互動式用戶端階段作業
{: #linux-run}

若要啟動用戶端，請執行下列指令：

```
cd /opt/ibm/securegateway/client
node lib/secgwclient.js <gateway ID> -t <security token>
```
{: codeblock}

通常，它會接受兩個參數：{{site.data.keyword.SecureGateway}} 閘道 ID 及閘道的安全記號，這兩者都可以透過「{{site.data.keyword.SecureGateway}} 儀表板」取得。

回到[開始使用 - 新增用戶端](/docs/services/SecureGateway/securegateway_client.html)。

## Windows
{: #windows}

### 安裝用戶端
{: #windows-install}

視系統的安全設定而定，您可能需要有管理專用權才能執行此安裝。

 1. 將 EXE 安裝程式檔案複製到您的系統。也會提供 MD5 總和，可用來檢查安裝套件的完整性。

 2. 它的安裝方式是按兩下它並回答提示，或從命令提示字元執行執行檔。

 3. 系統將提示使用者選擇他們選擇的安裝目錄。預設位置是 Program Files (x86) 目錄。

 4. 系統將提示使用者選取要用來啟動指令行介面的語言。如果未選取語言，則會預設為英文。

 5. 系統將提示使用者將用戶端安裝為 Windows 服務。如果使用者選擇這樣做，將會在背景中以 Windows 服務方式執行用戶端，並搭配使用者在後續對話框中提供的配置。可在「控制台」的服務頁面中檢查服務的狀態。服務名稱為 IBM Bluemix Secure Gateway Service。

 6. 安裝程式會識別機器上是否存在現有的安裝。如果偵測到，便會詢問使用者是否要使用現有配置，否則使用者可以選擇輸入新配置。這裡可以為每個閘道提供閘道 ID、安全記號、ACL 檔案及記載層次這類詳細資料。如果使用者選擇以 Windows 服務方式執行用戶端，則用戶端會搭配所提供的配置啟動。如果使用者未選擇以 Windows 服務方式啟動用戶端，則會儲存配置以供未來使用。不論是哪一種情況，配置都會儲存在 %Installation_directory%/ibm/securegateway/client/securegw_service.config 中。

 7. 從 1.5.0 版開始，用戶端具有功能完整的用戶端使用者介面。您可以從安裝程式本身進行配置。使用者可以提供密碼，以及要從中啟動用戶端使用者介面的埠號（預設值為 9003）。

 如果使用者選擇使用與多個閘道的連線來啟動用戶端，則請小心提供配置值。閘道 ID 需要以空格區隔。安全記號、ACL 檔案及記載層次應該以 -- 定界。如果您不想為特定閘道提供這三個值中的任何一個，請使用「無」。

 <b>附註：</b>請確定沒有任何剩餘的空格。

 <b>附註：</b>請注意，ACL 檔案應該放在 %Installation_directory%/ibm/securegateway/client 目錄或該路徑的相對路徑中。

### 啟動互動式用戶端階段作業
{: #windows-run}

若要啟動用戶端，請使用管理專用權開啟命令提示字元，然後執行下列指令：

```
cd "<Installation_directory>\ibm\securegateway\client"
secgw.cmd
```
{: codeblock}

或者，您也可以導覽至 `<Installation_directory>\ibm\securegateway\client`，然後按兩下 `secgw.cmd`。

<b>附註：</b>您可以選擇使用 `<Installation_directory>\ibm\securegateway\client\securegw_service.config` 檔案中所儲存的配置，或以互動方式提供詳細資料。

回到[開始使用 - 新增用戶端](/docs/services/SecureGateway/securegateway_client.html)。

## DataPower
{: #datapower}

DataPower 具有內嵌版的「{{site.data.keyword.SecureGateway}} 用戶端」。視 DataPower 版本而定，您可能有不同的「{{site.data.keyword.SecureGateway}} 用戶端」版本。請注意任何適用的 [DataPower 用戶端限制](/docs/services/SecureGateway/securegateway_interaction.html#limits-datapower)。使用舊的「Secure Gateway 用戶端」可能會遇到非預期的錯誤。

| DataPower 版本 | {{site.data.keyword.SecureGateway}} 用戶端版本 |
| -- | --  |
| 7.2.0.0、7.5.0.0 | 1.1.0  |
| 7.5.1.0、7.7.0 | 1.4.2  |
| 7.5.2.4 | 1.6.1  |
| 7.5.2.6、7.6.0.0 | 1.7.0  |
| 7.5.2.14、7.6.0.7、7.7.1.0 |  1.8.0fp6  |

### 啟動用戶端階段作業。
{: #datapower-run}

1. 登入 DataPower Web GUI。
2. 導覽至「物件」->「雲端」->「Secure Gateway 用戶端」，或搜尋「Secure Gateway 用戶端」。
3. 按一下`新增`，以配置新的用戶端連線。
4. 提供「名稱」、「閘道 ID」及「安全記號」（適用時），然後套用變更。

回到[開始使用 - 新增用戶端](/docs/services/SecureGateway/securegateway_client.html)。
