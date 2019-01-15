---

copyright:
  years: 2015, 2018
lastupdated: "2018-11-08"

---

# 常見問題

## OSI 模型層
{: #osi}

### 問題
Secure Gateway 服務代表 OSI 模型的哪一層？

### 回答
Secure Gateway 服務代表 OSI 模型的第 4 層。

## TLS 版本支援
{: #tls}

### 問題
Secure Gateway 服務支援哪個 TLS 版本？

### 回答
Secure Gateway 服務支援 TLS 1.2 版。

## 為什麼要停用閘道或目的地？
{: #disabled}

### 問題

為什麼要停用閘道或目的地？

### 回答
您可能會因為下列其中一個原因而想要停用目的地或閘道：

- 您建立了目的地，但尚未設定安全。在此情況下，您可能會在設定目的地的安全之前先停用目的地。
- 您不想要讓使用者能夠使用服務，因為您正在對服務進行一些更新。在此情況下，您可能會暫時停用必要的閘道，並等待更新服務。
- 您已設定前端的所有閘道及目的地，但仍在建置後端。在此情況下，您將在後端建置完成之前先停用閘道或目的地。

如需停用閘道或目的地的相關資訊，請參閱[如何管理 Secure Gateway 服務實例](./securegateway_managing.html)。

## 在多個空間之間建立自動化時，建議採用哪種方法？
{: #automation-spaces}

### 問題
客戶環境有一個組織及三個空間。一個空間適用於開發、另一個空間適用於編譯打包，而最後一個空間適用於正式作業。客戶應該建立單一或多個 Secure Gateway 實例（例如，一個空間一個實例）。如果客戶可以建立多個閘道，在每一個空間中重複使用 Node.js 應用程式來建立閘道及目的地時，是否有任何考量事項？

### 回答

- 您可以為全部三個空間建立單一 Secure Gateway 實例。不過，您必須記住[特定方案的閘道及目的地限制](./securegateway_plans.html)。
- 重複使用 Node.js 應用程式時沒有其他考量事項，因為 Secure Gateway 不需要任何服務連結。


## 在多個組織之間建立自動化時，建議採用哪種方法？
{: #automation-orgs}

### 問題
客戶環境有三個組織：一個適用於開發、一個適用於編譯打包、一個適用於正式作業。每個組織是否都需要 Secure Gateway 服務實例，以及配置是否可供該組織內的所有空間使用？

### 回答

- 您不需要在每個組織中都具有 Secure Gateway 服務實例。您可以在某個組織中有一個實例，然後從所有其他環境使用該實例內的閘道。使用此設定時，您必須記住[特定方案的閘道及目的地限制](./securegateway_plans.html)。
- 您可以在每個組織中具有一個 Secure Gateway 服務實例，配置將可供您的所有空間使用。

## 我的應用程式需要位於相同的空間嗎？
{: #app-space}

### 問題
是否需要在與 Secure Gateway 服務相同的 {{site.data.keyword.Bluemix_notm}} 空間中執行 Node.js 應用程式？

### 回答
否，您不需要在與 Secure Gateway 服務相同的 {{site.data.keyword.Bluemix_notm}} 空間中執行應用程式。

## 是否可以取得 Secure Gateway 伺服器日誌？
{: #server-logs}

### 問題
是否可以擷取 Secure Gateway 伺服器的錯誤日誌？

### 回答
無法擷取伺服器上的錯誤日誌。只能看到要求時所發生的錯誤。

## Secure Gateway 的功能狀態為何？
{: #states}

### 問題
閘道及目的地的不同生命週期狀態為何？

### 回答

#### 非功能狀態
1.7.0 版引進新的分層式方案定價模型。使用此模型，可以將「閘道」及「目的地」標示為「作用中」或「非作用中」。新的方案計費結構有一個部分會向使用者收取他們擁有之閘道及目的地數目的費用。

- 非作用中項目不會計費
- 「非作用中目的地」沒有已佈建的雲端埠。
- 非作用中閘道內的所有目的地也會處於非作用中狀態，而且在其閘道也設定為「作用中」之前，無法予以重新啟動。
- 非作用中項目會被視為「非功能」。非作用中項目不能處於任何功能狀態。

#### 功能狀態
將閘道或目的地標示為作用中時，將會對其進行計費。閘道及目的地的作用中狀態如下：

- 已啟用 - 閘道或目的地已就緒，可在正常情況下使用。
- 已停用 - 已停用閘道或目的地。
    - 閘道 - 用戶端無法讓資料流向已停用的閘道。
    - 目的地 - 用戶端無法建立與這些已停用目的地的連線。

## 如何知道「Secure Gateway 用戶端」上的資料活動？
{: #data-size}

### 問題
如何知道通過「Secure Gateway 用戶端」的資料活動？

### 回答
在「SecureGateway 用戶端」上，將記載層次變更為 TRACE。在傳送要求之後，將會顯示下列資訊。

從要求應用程式傳送的資料大小：
```
[TRACE] Connection #<connection ID> transmitted data: <size> bytes
```

從目的地傳送的資料大小：
```
[TRACE] Connection #<connection ID> received data: <size> bytes
```

## 如何在 Secure Gateway 上取得即時連線數？
{: #connect-num}

### 問題
如何從「Secure Gateway 用戶端」取得即時連線資訊、傳送的資料大小及接收的資料大小？

### 回答

- 在 Secure Gateway 用戶端互動式指令行上，
鍵入 `s`，以列印連線狀態詳細資料。 
- 在 Secure Gateway 用戶端使用者介面上，按一下「連線資訊」功能表。

即會顯示下列連線統計資料： 
- 已傳輸的整體資料大小。
- 已收到的整體資料大小。
- 連線總數。
- 即時作用中連線。

附註：不會即時呈現 Secure Gateway 使用者介面上的「現行連線」數目。請在 Secure Gateway 用戶端上使用上述方法，來擷取即時連線資訊。

## 有哪些建議配置可以讓我的連線更安全？
{: #secure-app}

### 問題
有哪些建議配置可以讓我的連線更安全？

### 回答

#### 使用交互鑑別
為內部部署目的地的兩端都啟用「交互鑑別」能讓 Secure Gateway 更安全。在「使用者鑑別」端，請啟用交互鑑別來限制對 Secure Gateway 雲端節點的存取，方法是在要求是透過 TLS/HTTPS 傳送時，使用用戶端憑證來進行鑑別。在「資源鑑別」端，請啟用交互鑑別以在連接至目的地端點時提供適當的認證，以確保對內部部署資源的安全/加密存取。如需相關資訊，請參閱[配置交互鑑別](./securegateway_destination.html#mutual-auth)及 [Node.js TLS 交互鑑別](./securegateway_tls-ma.html#node-js-tls-mutual-authentication)。

#### 設定 IP 表格規則（適用於內部部署目的地）
內部部署目的地的 Secure Gateway 雲端主機及埠位於公用空間中；因此，依預設會容許每個人進行存取。
若要控制 Secure Gateway 上的資料流量存取，請將 iptables 規則設為僅容許特定範圍的 IP 及埠進行存取，以保護內部部署資源的安全。如需如何在 Secure Gateway 上配置 iptables 規則的相關資訊，請參閱 [IP 表格規則](./securegateway_destination.html#configuring-network-security)。

#### 配置存取控制清單（適用於內部部署目的地）
配置「存取控制清單」支援以容許或限制對內部部署資源的存取，會透過在特定目的地主機及埠上指定存取權限，讓內部部署目的地更為安全。建議在 ACL 項目上也定義容許或限制 HTTP/S 路徑，以加強內部部署目的地的安全。如需相關資訊，請參閱[存取控制清單](./securegateway_acl.html#access-control-list)及[使用 ACL 的 HTTPS/路徑控制](./securegateway_acl.html#routes)。

#### 在 Secure Gateway 用戶端使用者介面上設定密碼
建議設定使用者介面密碼，以限制對「Secure Gateway 用戶端」使用者介面的存取。如需有關如何在「Secure Gateway 用戶端」終端機指令行上使用啟動配置或互動式指令來設定密碼的詳細資料，請參閱[與用戶端互動](./securegateway_interaction.html#interacting-with-the-client)。
