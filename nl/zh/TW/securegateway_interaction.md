---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-19"

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 與用戶端互動
{: #client-interacting}

有幾種方法可以與用戶端互動：

- 啟動之前，透過終端機指令行
- 啟動之後，使用用戶端的互動式指令行
- 啟動之後，使用用戶端的本端使用者介面

## 啟動配置
{: #startup}

### 啟動引數及選項
{: #startup-args}

下表說明可以和用戶端啟動指令一起提供的所有可用選項。使用這些項目，可容許用戶端在啟動之前完成用戶端配置，而不必在用戶端執行時進行個別設定。

|參數及引數|說明|
| ------------- | ----------- |
| &lt;gateway ID&gt; |使用所提供的閘道 ID 連接至 {{site.data.keyword.Bluemix_notm}}|
| -F, -\-aclfile &lt;file&gt; |存取控制清單檔案|
| -g, -\-gateway &lt;hostname:port&gt; |用來手動選取特定的閘道目的地（僅限進階使用）|
| -l, -\-loglevel &lt;level&gt; |將記載層次變更為 ERROR、INFO、DEBUG 或 TRACE|
| -p, -\-logpath &lt;file&gt; |直接記載至特定檔案|
| -t, -\-sectoken &lt;security token&gt; |用於此閘道連線的安全記號|
| -P, -\-port &lt;port&gt; |使用者介面執行所在的埠。預設為埠 9003 |
| -w, -\-password &lt;password&gt; | 要用來保護使用者介面的密碼。預設為無密碼|
| -x, -\-proxy &lt;proxy agent&gt; |埠 9000 連線的 Proxy 代理程式|
| -\-noUI |避免使用者介面自動啟動|
| -\-allow |允許對用戶端的所有連線。會被提供的 ACL 檔案所置換|
| -\-service |起始連線之後，母項會在所有子項用戶端都終止之後 60 秒內重新啟動|

<b>附註：</b>`--service`、`--allow` 及 `--noUI` 旗標應該是指令行引數中的最後幾個參數。

最基本的使用案例是以具有預設設定的單一用戶端連線開始：

```
<gateway_id> -t <security_token>
```
{: pre}

也可以延伸這些選項，以支援多個用戶端的自動連線。為了能夠將這些項目傳遞給多個用戶端連線，必須使用特定格式將它們傳入。傳遞閘道 ID 時可以使用與單一用戶端連線相同的方式（每個閘道 ID 以空格區隔）；不過，這些 ID 的順序會決定其餘引數必須遵循的順序。傳入任何其他引數時，必須以 `--` 予以區隔才能正確地讀取引數。如果未針對特定旗標傳入任何項目，則會假設它不套用至任何用戶端。

如果提供的引數不足以滿足所有閘道 ID，則會依序套用它們，直到沒有其他引數為止。例如，如果傳入兩個閘道 ID，並且傳入單一安全記號，則會將記號套用至第一個閘道 ID，而不是第二個。  

如果提供的閘道 ID 需要不同的引數，則應該在閘道未強制執行/提供的任何特定引數之處指定關鍵字 `none`。例如，如果傳入三個閘道 ID，而且使用者想要指定第一個及第三個的 loglevel，則引數將類似 `--loglevel DEBUG--none--TRACE`。在此情況下，none 便會預設為 INFO。

### 啟動引數範例
{: #startup-examples}

單一用戶端連線、自訂 loglevel、無使用者介面：

```
node lib/secgwclient.js <gateway_id> -t <security_token> -l <loglevel> --noUI
```
{: pre}

多個用戶端連線、預設選項：

```
node lib/secgwclient.js <gateway_id_1> <gateway_id_2> -t <security_token_1>--<security_token_2>
```
{: pre}

多個用戶端連線、所有自訂設定：

```
node lib/secgwclient.js <myGatewayID_1> <myGatewayID_2> -t none--<token for gateway 2> -l DEBUG--TRACE -p <full path to log file for gateway 1>--<full path to log file for gateway 2> -F <full path to ACL file for gateway 1>
```
{: pre}

回到[開始使用 - 新增用戶端](/docs/services/SecureGateway?topic=securegateway-add-client)。

## 互動式配置
{: #interactive}

### 互動式指令
{: #interactive-commands}

{{site.data.keyword.SecureGateway}} 用戶端具有指令行介面 (cli) 及 Shell 提示，可輕鬆地進行配置及控制。互動式環境所支援的功能集比指令行引數更豐富，這是為了協助對用戶端進行更佳的互動式控制。

|互動式指令|說明|
| ------------- | ----------- |
| A、acl allow &lt;hostname:port&gt; &lt;worker ID&gt; |容許存取控制清單|
| D、acl deny &lt;hostname:port&gt; &lt;worker ID&gt; |拒絕存取控制清單|
| N、no acl &lt;hostname:port&gt; &lt;worker ID&gt; |移除存取控制清單項目|
| S、show acl &lt;worker ID&gt; |顯示所有存取控制清單項目|
| F、acl file &lt;file&gt; &lt;worker ID&gt; |存取控制清單檔案|
| C、displayconfig &lt;worker ID&gt; |顯示現行 {{site.data.keyword.SecureGateway}} 配置（如果可用的話）|
| a、authorize &lt;worker ID&gt; |切換指定工作者節點的出埠 TLS 連線 rejectUnauthorized 參數置換|
| t、sectoken &lt;security token&gt; |要用於下一個閘道連線的安全記號|
| c、connect &lt;gateway ID&gt; |使用所提供的閘道 ID 連接至 {{site.data.keyword.Bluemix_notm}}|
| l、loglevel &lt;level&gt; &lt;worker ID&gt; |將記載層次變更為 ERROR、INFO、DEBUG 或 TRACE|
| p、logpath &lt;file&gt; &lt;worker ID&gt; |直接記載至特定檔案|
| r、reverse &lt;worker ID&gt; |列出用戶端目前接聽反向目的地的埠|
| k、kill &lt;worker ID&gt;  |終止指定的工作者節點|
| e、select &lt;worker ID&gt;  |除非另行指定，否則會指定要對其執行指令的工作者節點|
| d、deselect |取消選取先前指定的工作者節點。發出 select 指令，以指定另一個工作者節點|
| w、password &lt;old password&gt; &lt;new password&gt; |設定使用者介面密碼。如果 &lt;new password&gt; 為空白，則不會強制使用密碼。密碼更新時需要有 &lt;old password&gt;。密碼必須只包含字母|
| P、port &lt;new port&gt; |變更使用者介面所接聽的埠|
| u、uistart &lt;initial password&gt; &lt;port&gt; |在 localhost:&lt;port&gt;/dashboard 上啟動使用者介面。如果 &lt;initial password&gt; 為空白，且尚未針對階段作業設定任何其他密碼，則不會強制使用使用者介面密碼。如果 &lt;port&gt; 為空白，則可在 9003 聯繫使用者介面|
| U、uistop |關閉與此用戶端階段作業相關聯的使用者介面。在手動啟動新的使用者介面之前，此階段作業只能透過 CLI 存取|
| R、revoke |清除與此用戶端階段作業相關聯的所有使用者介面授權|
| q、quit |中斷連線並結束|
| s、status &lt;worker ID&gt;  |列印通道及其連線的狀態詳細資料|
| L、list |顯示目前相關聯工作者節點的清單|

<b>附註：</b>如果已使用 `select` 指令指定連線，並在未提供工作者節點 ID 的情況下呼叫另一個指令，則會嘗試對 `select` 所指定的連線執行指令。

如需配置「存取控制清單」的詳細資料，[請按一下這裡](/docs/services/SecureGateway?topic=securegateway-acl)。

回到[開始使用 - 新增用戶端](/docs/services/SecureGateway?topic=securegateway-add-client)。

## 用戶端使用者介面
{: #client-ui}

<b>附註：</b>在 Windows 或 MacOS 上使用 Docker 時，不支援「用戶端使用者介面」。

用戶端使用者介面提供本端 Web 介面（而不是 CLI）讓使用者與「{{site.data.keyword.SecureGateway}} 用戶端」進行互動。依預設，此使用者介面提供於 `localhost:9003/dashboard`。使用者介面分割為下列幾個頁面：

### 連接
{: #ui-connect}

這是使用者介面的起始登陸頁面，使用者可以在這裡提供閘道 ID 及安全記號，以連接其第一個用戶端。

### 登入
{: #ui-login}

如果使用者介面受到密碼保護，則會顯示此頁面。如果未強制使用密碼即到達此頁面，請重新整理頁面，以便重新導向。

### 儀表板
{: #ui-dashboard}

這是用戶端已連接之後的主頁面。從這裡，您可以存取「檢視日誌」頁面、「存取控制清單」頁面及「連線資訊」頁面。在底端，您也可以選擇中斷與某個/多個已連接用戶端的連線。在頁面頂端，會顯示目前已選取的用戶端，且會有用來連接其他用戶端的選項。 

### 檢視日誌
{: #ui-logs}

此頁面可讓您查看選取之用戶端（顯示在頁面右上方）所產生的日誌。可以透過日誌下面的勾選框來過濾顯示的日誌。

### 存取控制清單
{: #ui-acl}

此頁面可讓您操作選取之用戶端（顯示在頁面右上方）的「存取控制清單」。規則可以個別新增至容許/拒絕表格，或者可以在頁面底端上傳檔案。

### 連線資訊
{: #ui-info}

此頁面將顯示選取之用戶端（顯示在頁面右上方）的現行連線資訊。在這裡可以看到閘道說明、現行連線數及反向目的地接聽器之類的資訊。

回到[開始使用 - 新增用戶端](/docs/services/SecureGateway?topic=securegateway-add-client)。

## 遠端用戶端終止
{: #client-remote}

如果已提供用戶端的 ID，則可以透過 SG 使用者介面或 SG API 從遠端終止用戶端。如果您終止正在以服務方式執行的用戶端，則用戶端會重新啟動，並取得新的用戶端 ID；不過，如果服務已有多個連接的用戶端，則在所有剩餘用戶端都被終止之前，不會重新啟動終止的用戶端。

## 限制
{: #limits}

### 連線限制
{: #limits-conn}

SG 閘道只能處理 250 個並行連線。如果並行要求數目超出限制，則可能會導致連線嘗試遭到拒絕，而且會導致延遲。修正此問題的一個簡單方法，就是在呼叫端應用程式上使用連線儲存區。請注意，250 個並行連線的限制是針對閘道，而不是用戶端或目的地。將在閘道的所有用戶端與目的地之間共用此限制。

### DataPower Client 限制
{: #limits-datapower}

{{site.data.keyword.SecureGateway}} DataPower Client 正在進行更新，以符合其餘用戶端的功能。它目前有下列限制：

- ACL 將預設為 ALLOW ALL
- 不支援從 {{site.data.keyword.SecureGateway}} 使用者介面啟用及停用閘道或目的地。不過，DataPower 使用者介面中的「管理狀態」選項，其功能等同於該特定用戶端的開關。
- 不支援連線狀態輪詢，無法取得即時連接及斷線的閘道狀態更新。
- DataPower 7.5.1.0 版之前不支援具有目的地端 TLS 的完整憑證鏈
- DataPower 7.5.1.0 版之前不支援雲端目的地
- 記載層次無法變更為 TRACE 層次
- DataPower 中最新的 Secure Gateway 用戶端版本是 1.8.2fp1，如需相關資訊，請檢查[這裡](/docs/services/SecureGateway?topic=securegateway-client-install#installing-datapower)。
