---

copyright:
  years: 2015, 2017
lastupdated: "2017-04-10"

---

# 新增目的地
{: #add-dest}

目的地是如何連接至特定內部部署或雲端資源的定義。建立目的地之後，{{site.data.keyword.SecureGateway}} 伺服器會提供它一個唯一公用端點，它會在閘道連接時在該處接聽連線。

![沒有目的地的儀表板](./images/emptyDestinations.png?raw=true "沒有目的地的儀表板")

從新的閘道內及「目的地」標籤上，按一下「新增目的地」按鈕，以開啟「新增目的地」畫面。建立目的地有兩種方法：引導式設定會顯示每個部分如何妥善納入整體架構，進階設定則會在單一畫面上提供所有欄位及選項。  

引導式設定<b>不</b>容許配置 Proxy 資訊、伺服器名稱指示器，或上傳目的地特有的憑證/金鑰配對。建立之後，可以透過「編輯目的地」畫面來使用所有欄位。

## 引導式設定畫面
{: #add-dest-guided-setup}

![引導式設定](./images/guidedLanding.png?raw=true "「引導式設定」登陸畫面")

## 進階設定畫面
{: #add-dest-advanced-setup}

![進階設定](./images/advancedLanding.png?raw=true "「進階設定」登陸畫面")

## 內部部署目的地與雲端目的地
{: #dest-types}

建立目的地時，第一個要回答的問題，就是您需要連接的資源位在何處。

### 內部部署目的地
{: #dest-types-on-prem}
內部部署目的地適用的使用案例中，公用空間中的應用程式需要存取位於內部部署的受限資源。
![內部部署目的地](./images/onPremDestination.png?raw=true "內部部署目的地")

### 雲端目的地
{: #dest-types-on-cloud}
雲端目的地適用的使用案例中，位在受限網路中的應用程式需要存取公用空間中可用的資源。
![反向目的地](./images/reverseDestination.png?raw=true "雲端目的地")

## 定義目的地
{: #define-dest}
對於這兩種目的地類型，都需要下列資訊：

- <b>資源主機名稱</b>：這是您需要連接之資源的 IP 或主機名稱。
- <b>資源埠</b>：這是您的資源接聽的埠。
- <b>通訊協定</b>：這是您的應用程式將建立的連線類型。請參閱下表，以取得各種[通訊協定選項](#protocols-options)。若要配置您資源所預期的連線類型，請參閱[資源鑑別](#dest-resource-auth)小節。

如果您已選取雲端目的地，則還需要提供<b>用戶端埠</b>。這是「{{site.data.keyword.SecureGateway}} 用戶端」接聽的埠，以容許連線到關聯的資源主機名稱及埠。

## 通訊協定選項
{: #protocols-options}

此表格提供所有可用的選項，供您的應用程式使用 {{site.data.keyword.SecureGateway}} 來起始連線/要求。

通訊協定|說明
-- | --
TCP| 無鑑別。應用程式可以直接與 {{site.data.keyword.SecureGateway}} 伺服器進行通訊，而不需要任何憑證。
TLS：伺服器端| 已啟用「傳輸層安全 (TLS)」，且伺服器提供憑證來證明其確實性。
TLS：交互鑑別| 伺服器提供一組憑證，而且您的應用程式也必須提供憑證作為其連線的一部分。
HTTP| TCP 連線，其中已重新編寫主機標頭，以符合內部部署主機名稱。
HTTPS|「TLS：伺服器端」連線，其中已重新編寫主機標頭，以符合內部部署主機名稱。
HTTPS：交互鑑別|「TLS：交互鑑別」連線，其中已重新編寫主機標頭，以符合內部部署主機名稱。


## 配置交互鑑別
{: #dest-mutual-auth}

對於強制執行交互鑑別的通訊協定，您將需要上傳自己的憑證，否則伺服器將自動建立自簽憑證/金鑰配對，供您的應用程式使用。此配對可以與伺服器憑證一起下載。
![「交互鑑別」畫面](./images/mutualAuth.png?raw=true "「交互鑑別」畫面")

### 使用者鑑別
{: #dest-user-auth}

使用者鑑別區段適用於使用 {{site.data.keyword.SecureGateway}} 來管理要求端/連接端應用程式的授權。此欄位接受的單一憑證應該是您的應用程式將與任何連線/要求一起提出的憑證。

### 資源鑑別
{: #dest-resource-auth}

資源鑑別決定 {{site.data.keyword.SecureGateway}} 如何嘗試連接至已定義的資源。有三個可用的選項：無、TLS（伺服器端）及交互鑑別。根據您的選擇，會有不同的鑑別選項變成可用。

在您的資源連線上啟用 TLS 與用於「使用者鑑別」的 TLS 不同。用於「使用者鑑別」的 TLS 可保護起始要求端應用程式與 {{site.data.keyword.SecureGateway}} 之間的存取（例如 {{site.data.keyword.Bluemix_notm}} 應用程式與 {{site.data.keyword.SecureGateway}} 伺服器之間），而用於「資源鑑別」的 TLS 則保護 {{site.data.keyword.SecureGateway}} 與已定義資源之間的連線（例如 {{site.data.keyword.SecureGateway}} 用戶端與內部部署資料庫之間）。

#### 雲端/內部部署鑑別
{: #cloud-or-on-prem-auth}

針對[資源鑑別](#dest-resource-auth)選取 TLS 或「交互鑑別」，即可使用此選項。欄位的名稱將符合您所選擇的[目的地類型](#dest-types)。此欄位最多可容許上傳 6 個憑證，以驗證您要連接之資源的憑證。這些檔案將新增至資源連線的 CA，而且應該包含您的資源將提出的憑證或憑證鏈。

#### 伺服器名稱指示器 (SNI)
{: #dest-sni}
針對[資源鑑別](#dest-resource-auth)選取 TLS 或「交互鑑別」，即可使用此選項。這用來容許提供不同的主機名稱以進行資源連線的 TLS 信號交換。

### 用戶端憑證及金鑰
{: #dest-client-cert-key}
「用戶端憑證」及「金鑰」欄位的出現位置，取決於您選擇的[目的地類型](#dest-types)。在這兩種情況下，「SG 用戶端」會使用這裡提供的檔案來識別它自己以進行 TLS 連線。如果未上傳任何檔案，則 {{site.data.keyword.SecureGateway}} 伺服器會自動產生具有 CN `localhost` 的自簽配對。如需如何產生憑證/金鑰配對的指示，[請按一下這裡](/docs/services/SecureGateway/securegateway_keygen.html)。

若為內部部署目的地，如果已選取`資源鑑別：交互鑑別`，則這會出現在[資源鑑別](#dest-resource-auth)下。在此情況下，SG 用戶端會將此憑證/金鑰配對用於它對已定義資源的出埠連線，內部部署資源需要將此憑證新增至它的 CA 才能與 SG 用戶端通訊。

若為雲端目的地，如果已選取 TLS 通訊協定，則這會出現在[使用者鑑別](#dest-user-auth)下。在此情況下，SG 用戶端會將此憑證/金鑰配對用於建立 TLS 接聽器，內部部署應用程式需要將此憑證新增至它的 CA 才能與 SG 用戶端通訊。

## 配置網路安全
{: #dest-network-security}
若要防止除了特定 IP 位址以外的所有 IP 位址連接至雲端主機及埠，您可以選擇在內部部署目的地上強制執行 iptables 規則。
![「網路安全」畫面](./images/networkSecurity.png?raw=true "「網路安全」畫面")

若要強制執行 iptables 規則，請從「網路安全」畫面中勾選<b>使用 iptables 規則來限制對此目的地的雲端存取</b>方框。勾選該方框之後，即可開始新增應該容許連接的 IP。如果未提供任何 IP，則只要勾選<b>限制雲端存取</b>方框，就會拒絕對此雲端主機及埠的所有連線。

<b>附註</b>：提供的 IP 或埠必須是 {{site.data.keyword.SecureGateway}} 伺服器看到的外部 IP 位址，而不是提出要求之機器的本端 IP 位址。

### 新增 iptables 規則
{: #dest-iptables}
將規則新增至 iptables 時，您可以提供個別 IP 或 IP 範圍，以及單一埠或埠範圍。提供的所有範圍都是包含頭尾數值。下表具有一些範例，以及它們在 iptables 內的解析方式：

IP 位址 | 埠 | 結果
-- | -- | --
1.2.3.4 | 5000 | 僅容許來自埠 5000 的 IP 1.2.3.4。
1.2.3.4-2.3.4.5 | 5000 |容許來自埠 5000 且在 1.2.3.4 與 2.3.4.5 之間的所有 IP。
1.2.3.4 | 5000:10000 | 僅容許來自埠 5000 到 10000 的 IP 1.2.3.4。
1.2.3.4-2.3.4.5 | 5000:10000 |容許來自埠 5000 到 10000 且在 1.2.3.4 與 2.3.4.5 之間的所有 IP。
1.2.3.4 | | 僅容許來自任何埠的 IP 1.2.3.4。
| 5000 | 容許來自埠 5000 的任何 IP。

特定規則也可以與應用程式相關聯。如需建立關聯規則的相關資訊，請參閱[如何為應用程式建立 iptables 規則](/docs/services/SecureGateway/iptables.html)。

## 配置 Proxy 選項
{: #dest-proxy}
如果您的內部部署目的地位於 SOCKS Proxy 後面，則您可以在「Proxy 選項」畫面中配置目的地的 Proxy 設定。
![「Proxy 選項」畫面](./images/proxyOptions.png?raw=true "「Proxy 選項」畫面")

若要配置 Proxy 設定，您只需要提供 Proxy 接聽的主機名稱及埠，以及所使用的 SOCKS 通訊協定（4、4a、5）。

## 目的地設定
{: #dest-settings}
建立目的地之後，請按一下設定圖示，以查看下列資訊：

- 使用 API 所需的目的地 ID。
- <b>內部部署目的地將會有雲端主機及埠。您的應用程式需要有此資訊，才能連接至內部部署資源。</b>
- <b>雲端目的地將會有用戶端埠。這是「{{site.data.keyword.SecureGateway}} 用戶端」將接聽的埠，以將內部部署應用程式連接至雲端資源。</b>
- 此目的地所指向的資源主機及埠。
- 目的地建立時間，以及建立它的使用者。
- 目的地前次修改時間，以及修改它的使用者。
