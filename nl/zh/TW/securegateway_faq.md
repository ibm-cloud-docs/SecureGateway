---

copyright:
  years: 2015, 2019
lastupdated: "2019-04-29"

---

# 常見問題
{: #sg-faq}

## OSI 模型層
{: #faq-osi}

### 問題
{: #osi-question}
Secure Gateway 服務代表 OSI 模型的哪一層？

### 回答
{: #osi-answer}
Secure Gateway 服務代表 OSI 模型的第 4 層。

## TLS 版本支援
{: #faq-tls}

### 問題
{: #tls-question}
Secure Gateway 服務支援哪個 TLS 版本？

### 回答
{: #tls-answer}
Secure Gateway 服務支援 TLS 1.2 版。

## 為什麼要停用閘道或目的地？
{: #faq-disabled}

### 問題
{: #disabled-question}

為什麼要停用閘道或目的地？

### 回答
{: #disabled-answer}
您可能會因為下列其中一個原因而想要停用目的地或閘道：

- 您建立了目的地，但尚未設定安全。在此情況下，您可能會在設定目的地的安全之前先停用目的地。
- 您不想要讓使用者能夠使用服務，因為您正在對服務進行一些更新。在此情況下，您可能會暫時停用必要的閘道，並等待更新服務。
- 您已設定前端的所有閘道及目的地，但仍在建置後端。在此情況下，您將在後端建置完成之前先停用閘道或目的地。

如需停用閘道或目的地的相關資訊，請參閱[如何管理 Secure Gateway 服務實例](/docs/services/SecureGateway?topic=securegateway-manage-sg-service)。

## 在多個空間自動化建立作業時，建議採用哪種方法？
{: #faq-automation-spaces}

### 問題
{: #automation-spaces-question}
客戶環境有一個組織及三個空間。一個空間適用於開發、另一個空間適用於編譯打包，而最後一個空間適用於正式作業。客戶應該建立單一還是多個 Secure Gateway 實例（例如，一個空間一個實例）？如果客戶可以建立多個閘道，在每一個空間中重複使用 Node.js 應用程式來建立閘道及目的地時，是否有任何考量事項？

### 回答
{: #automation-spaces-answer}

- 您可以為全部三個空間建立單一 Secure Gateway 實例。不過，您必須記住[特定方案的閘道及目的地限制](/docs/services/SecureGateway?topic=securegateway-secure-gateway-service-plans)。
- 重複使用 Node.js 應用程式時沒有其他考量事項，因為 Secure Gateway 不需要任何服務連結。


## 在多個組織之間自動化建立作業時，建議採用哪種方法？
{: #faq-automation-orgs}

### 問題
{: #automation-orgs-question}
客戶環境有三個組織：一個適用於開發、一個適用於編譯打包、一個適用於正式作業。每個組織是否都需要 Secure Gateway 服務實例，以及配置是否可供該組織內的所有空間使用？

### 回答
{: #automation-orgs-answer}

- 您不需要在每個組織中都具有 Secure Gateway 服務實例。您可以在某個組織中有一個實例，然後從所有其他環境使用該實例內的閘道。使用此設定時，您必須記住[特定方案的閘道及目的地限制](/docs/services/SecureGateway?topic=securegateway-secure-gateway-service-plans)。
- 您可以在每個組織中具有一個 Secure Gateway 服務實例，配置將可供您的所有空間使用。

## 我的應用程式需要位於相同的空間嗎？
{: #faq-app-space}

### 問題
{: #app-space-question}
是否需要在與 Secure Gateway 服務相同的 {{site.data.keyword.Bluemix_notm}} 空間中執行 Node.js 應用程式？

### 回答
{: #app-space-answer}
否，您不需要在與 Secure Gateway 服務相同的 {{site.data.keyword.Bluemix_notm}} 空間中執行應用程式。

## 是否可以取得 Secure Gateway 伺服器日誌？
{: #faq-server-logs}

### 問題
{: #server-logs-question}
是否可以擷取 Secure Gateway 伺服器的錯誤層次日誌？

### 回答
{: #server-logs-answer}
無法擷取伺服器上的錯誤層次日誌。只能看到要求時所發生的錯誤。

## Secure Gateway 的功能狀態為何？
{: #faq-states}

### 問題
{: #states-question}
閘道及目的地的不同生命週期狀態為何？

### 回答
{: #states-answer}

#### 非功能狀態
{: #states-answer-non-functional}
1.7.0 版引進新的分層式方案定價模型。使用此模型，可以將「閘道」及「目的地」標示為「作用中」或「非作用中」。新的方案計費結構有一個部分會向使用者收取他們擁有之閘道及目的地數目的費用。

- 非作用中項目不會計費
- 「非作用中目的地」沒有已佈建的雲端埠。
- 非作用中閘道內的所有目的地也會處於非作用中狀態，而且在其閘道也設定為「作用中」之前，無法予以重新啟動。
- 非作用中項目會被視為「非功能」。非作用中項目不能處於任何功能狀態。

#### 功能狀態
{: #states-answer-functional}
將閘道或目的地標示為作用中時，將會對其進行計費。閘道及目的地的作用中狀態如下：

- 已啟用 - 閘道或目的地已就緒，可在正常情況下使用。
- 已停用 - 已停用閘道或目的地。
    - 閘道 - 用戶端無法讓資料流向已停用的閘道。
    - 目的地 - 用戶端無法建立與這些已停用目的地的連線。

## 如何知道「Secure Gateway 用戶端」上的資料活動？
{: #faq-data-size}

### 問題
{: #data-size-question}
如何知道通過「Secure Gateway 用戶端」的資料活動？

### 回答
{: #data-size-answer}
在「Secure Gateway 用戶端」上，將記載層次變更為 TRACE。在傳送要求之後，將會顯示下列資訊。

從要求應用程式傳送的資料大小：
```
[TRACE] Connection #<connection ID> transmitted data: <size> bytes
```

從目的地傳送的資料大小：
```
[TRACE] Connection #<connection ID> received data: <size> bytes
```

## 如何在 Secure Gateway 上取得即時連線數？
{: #faq-connect-num}

### 問題
{: #connect-num-question}
如何從「Secure Gateway 用戶端」取得即時連線資訊，例如即時連線數、傳送的資料大小及接收的資料大小？

### 回答
{: #connect-num-answer}

- 在 Secure Gateway 用戶端互動式指令行上，
鍵入 `s`，以列印連線狀態詳細資料。 
- 在 Secure Gateway 用戶端使用者介面上，按一下「連線資訊」功能表。

即會顯示下列連線統計資料： 
- 已傳輸的整體資料大小。
- 已收到的整體資料大小。
- 連線總數。
- 即時作用中連線。

附註：此連線資訊是在用戶端層次，而非閘道層次。如果您需要閘道層次的連線資訊，請檢查連接至該閘道的每個用戶端。

## 有哪些建議配置可以讓我的連線更安全？
{: #faq-secure-app}

### 問題
{: #secure-app-question}
有哪些建議配置可以讓我的連線更安全？

### 回答
{: #secure-app-answer}

#### 使用交互鑑別
{: #secure-app-answer-ma}
為內部部署目的地的兩端都啟用「交互鑑別」能讓 Secure Gateway 更安全。在「使用者鑑別」端，請啟用交互鑑別來限制對 Secure Gateway 雲端節點的存取，方法是在要求是透過 TLS/HTTPS 傳送時，使用用戶端憑證來進行鑑別。在「資源鑑別」端，請啟用交互鑑別以在連接至目的地端點時提供適當的認證，並確保對內部部署資源的安全/加密存取。如需相關資訊，請參閱[配置交互鑑別](/docs/services/SecureGateway?topic=securegateway-add-dest#dest-mutual-auth)及 [Node.js TLS 交互鑑別](/docs/services/SecureGateway?topic=securegateway-nodejs-tls-ma#nodejs-tls-ma)。

#### 設定 IP 表格規則（適用於內部部署目的地）
{: #secure-app-answer-iptables}
內部部署目的地的 Secure Gateway 雲端主機及埠位於公用空間中；因此，依預設會容許每個人進行存取。若要控制 Secure Gateway 上的資料流量存取，請將 iptables 規則設為僅容許特定範圍的 IP 及埠進行存取，以保護內部部署資源的安全。如需如何在 Secure Gateway 上配置 iptables 規則的相關資訊，請參閱 [IP 表格規則](/docs/services/SecureGateway?topic=securegateway-add-dest#dest-network-security)。

#### 配置存取控制清單（適用於內部部署目的地）
{: #secure-app-answer-acl}
配置「存取控制清單」支援以容許或限制對內部部署資源的存取，會透過在特定目的地主機及埠上指定存取權限，讓內部部署目的地更為安全。建議在 ACL 項目上也定義容許或受限的 HTTP/S 路徑，以加強內部部署目的地的安全。如需相關資訊，請參閱[存取控制清單](/docs/services/SecureGateway?topic=securegateway-acl#acl)及[使用 ACL 的 HTTP/S 路徑控制](/docs/services/SecureGateway?topic=securegateway-acl#acl-route-control)。

#### 在 Secure Gateway 用戶端使用者介面上設定密碼
{: #secure-app-answer-ui-pw}
建議設定使用者介面密碼，以限制對「Secure Gateway 用戶端」使用者介面的存取。如需有關如何在「Secure Gateway 用戶端」終端機指令行上使用啟動配置或互動式指令來設定密碼的詳細資料，請參閱[與用戶端互動](/docs/services/SecureGateway?topic=securegateway-client-interacting#client-interacting)。

## 何謂閘道移轉？2018 年 12 月之後為何網域已變更？
{: #faq-gateway-migration}

### 問題
{: #gateway-migration-question}
2018 年 12 月的維護之後，在閘道畫面上有一個移轉按鈕，按鈕的作用為何？網域為何變更了？

### 回答
{: #gateway-migration-answer}

2018 年 12 月維護之後，Secure Gateway 的雲端主機開始重新命名為使用 `securegateway.appdomain.cloud`，而不使用 `integration.ibmcloud.com`，並且使用 `securegateway.cloud.ibm.com` 進行閘道鑑別而不使用 `bluemix.net`。為了舊版相容性，現有閘道將維持使用舊的網域，直到閘道移轉為止，且 SG 用戶端 v180fp9 及更早版本將維持使用 `bluemix.net` 進行閘道鑑別。

移轉之後，內部部署目的地的雲端主機將變更為使用新的網域，使用者/應用程式將需要更新為傳送要求到新的雲端主機。

目前移轉並不是強制性的，且沒有關於舊網域何時不再支援的確切日期，但一旦確定之後，仍在使用舊網域名稱的客戶將會收到通知。

## 可以在何處收到通知？
{: #faq-notification}

### 問題
{: #notification-question}
可以在何處收到 Secure Gateway 通知，尤其是針對干擾性的維護？

### 回答
{: #notification-answer}

您可以透過我們的[狀態頁面](https://cloud.ibm.com/status?selected=status)收到通知。
- 若要取得已完成/進行中且會造成中斷之維護的相關通知，請在 `Status` 標籤搜尋 `Secure Gateway`。
- 若要取得計劃性且會造成中斷之維護的相關通知，請在 `Planned maintenance` 標籤搜尋 `Secure Gateway`。

當 Secure Gateway 用戶端非預期地斷線時，請前往狀態頁面檢查是否在當時有干擾性維護。

如果維護需要中斷作業超過 10 分鐘，則您可能需要手動重新啟動 Secure Gateway 用戶端，以在維護之後重新連接至 Secure Gateway 伺服器。一般而言，服務關閉時間將等於或少於 10 分鐘，Secure Gateway 用戶端（在 v180 版之後）應該能夠自動重新連接至 Secure Gateway 伺服器。

## 如何擷取 DataPower 上的 Secure Gateway 用戶端日誌？
{: #faq-dp-log}

### 問題
{: #dp-log-question}
如何擷取 DataPower 上的 Secure Gateway 用戶端日誌，並將它寫入檔案？

### 回答
{: #dp-log-answer}

Secure Gateway 用戶端日誌的事件種類是 `sgclient`。請建立[日誌目標](https://www.ibm.com/support/knowledgecenter/en/SS9H2Y_7.7.0/com.ibm.dp.doc/logtarget_logs.html)，將具有特定事件種類的日誌寫入檔案，範例如下：

- 從預設領域：
    - GUI 側畫面，選取 `Object` → `Logging Configuration` → `Log Target`。或在 `Search` 欄位搜尋 `Log Target`。
    - 選取 `Add` 按鈕，以新增日誌目標。
- 在 `Main` 標籤中：
    - 填寫 `Name`
    - `Target Type` 為 `File`
    - `Log format` 為 `Text`
    - 填寫 `File Name` 以定義輸出位置，例如 `logtemp:///sgclient.log`
    - 將 `Archive Mode` 選取為 `Rotate`
- 在 `Event Subscription` 標籤中：
    - 填寫 `Name`
    - 選取 `Add` 按鈕，以新增目標事件訂閱
    - 填寫 `Event Category` 並選取 `sgclient`
    - 填寫 `Minimum Event Priority` 為 `debug`
