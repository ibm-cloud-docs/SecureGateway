---

copyright:
  years: 2015, 2018
lastupdated: "2018-08-10"

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 疑難排解
{: #troubleshooting}

## 執行 Secure Gateway 用戶端的最佳作法
{: #best-practices}

- 在用戶端本身所橋接之服務可以透過網路看到的作業系統 (OS) 分割區上執行 Secure Gateway 用戶端。例如，部分管理的虛擬化環境支援多個網路連線功能模式，包括 NAT 及橋接器。請務必選擇讓您能從網際網路存取 {{site.data.keyword.Bluemix}} 服務的正確連線類型。
- 將 Secure Gateway 安裝至您企業安全原則所容許的 IT 環境中。這通常會在受保護的黃色管制區或 DMZ 中，在這裡，貴公司可以制定適當的安全控制措施來保護內部部署資產。安裝 Secure Gateway 用戶端時，請一律遵循您的企業安全原則及指示。
- 在將用戶端安裝至您的環境之前，請確定網際網路及您的內部部署資產都是可存取的，且 DNS 可解析所有主機名稱。

## 起始疑難排解步驟
{: #initial-troubleshooting}

- 從要求端應用程式起始失敗的要求
- 檢查「Secure Gateway 用戶端」日誌
- 如果尚未從要求產生任何用戶端日誌，則該問題介於要求端應用程式與「Secure Gateway 伺服器」之間。其範圍可能會從網路可靠性、不相符的要求通訊協定，到不適當的 TLS 交互鑑別信號交換。
- 如果用戶端已從要求產生錯誤層次日誌，則問題介於「SG 用戶端」與內部部署資源之間。以下是包含一般錯誤的表格、一般造成它們的問題，以及用來進行疑難排解的潛在方法。

錯誤 | 一般原因 | 疑難排解方法
--- | --- | ---
ETIMEDOUT |由於網路限制，用戶端找不到要連接的主機名稱/IP。|嘗試從執行用戶端的主機對目的地的主機名稱/IP 進行連線測試，以確保網路連線功能。如果執行 Docker 版本的用戶端，則使用 `--net=host` 將容器與主機 OS 橋接在一起，可能會解決此問題。
ECONNREFUSED | 用戶端已解析要連接的主機名稱/IP，但無法開始連線信號交換 |這一般是「SG 用戶端」與內部部署資源之間的通訊協定不相符所導致（例如，用戶端嘗試使用 TCP 連線連接至預期使用 TLS 連線的 host:port）。在某些情況下，防火牆規則可能會造成此錯誤，而不是 ETIMEDOUT。
ECONNRESET | 用戶端已建立與目的地的連線，但在信號交換期間發生錯誤（TLS 信號交換錯誤也可能會導致不同的錯誤），或內部部署資源正在處理要求時發生錯誤。| 應該檢查內部部署資源的日誌，確認未發生任何錯誤導致連線中斷。如果在內部部署日誌中找不到任何內容，則應該檢查目的地配置，確保提供適當的通訊協定（並視需要提供憑證）給用戶端進行連線。
REMOTE_RST |「SG 伺服器」端發生錯誤。<br><br> 若為內部部署目的地，要求端應用程式連接至「SG 伺服器」時發生錯誤，或從內部部署資源接收資料時發生逾時錯誤。<br><br> 若為雲端目的地，可能是從 TLS 信號交換失敗到雲端資源發生錯誤的任何原因| 若為內部部署目的地，請確定要求端應用程式使用適當的通訊協定來建立與「SG 伺服器」的連線；如果從內部部署資源接收資料時發生錯誤，則請嘗試延長/停用逾時。<br><br> 若為雲端目的地，應該檢查雲端資源的日誌，確認未發生任何錯誤導致連線中斷。如果在雲端資源日誌中找不到任何內容，則應該檢查目的地配置，確保提供適當的通訊協定（並視需要提供憑證）給用戶端進行連線。

在通道的另一端發生 ECONNRESET 之後，許多應用程式都會「停滯」。這是預期狀況。Secure Gateway 無法在通道另一端重播 RST 封包，因為通道那端已確認 TCP 封包。在應用程式上定義逾時，使其絕不會收到確認回應，是結束停滯的唯一方法。

## 將 Docker 用戶端配置為在伺服器重新啟動時重新啟動
{: #docker-auto-restart}

### 發生的狀況
{: #docker-auto-restart-what-is-happening}
當您重新啟動 Secure Gateway 用戶端執行所在的伺服器時，必須手動重新啟動 Secure Gateway Docker 用戶端。如何讓用戶端在系統重新啟動之後自動啟動？

### 修正方式
{: #docker-auto-restart-how-to-fix-it}

- 在 Linux 或 UNIX 系統上：
- 將 Docker 指令整合到可以藉由 CRON 工作呼叫的 Script。
- 如果您使用 Linux 工作站，則可以建立 Upstart 配置，確保每次伺服器重新啟動時都會啟動用戶端。如需相關資訊，請參閱 Docker 網站中的 Automatically Start Containers。
- 在 Windows 系統上，發出下列指令來啟動用戶端：

```
for /L %i in (0,0,0) do docker run -it ibmcom/secure-gateway-client <gateway_id>
```
{: pre}

## 連線錯誤訊息：主機不在憑證的 CN 中
{: #not-in-cn}

### 發生的狀況
{: #not-in-cn-what-is-happening}
您嘗試使用 Secure Gateway 用戶端實作內部部署的用戶端 TLS，並且收到下列錯誤訊息。

```
[ERROR] Connection #<connection ID> had error: Host: <host name>. is not cert's CN: <mycommonname>

Where:
    - connection ID is a client assigned connection number.
    - host name is the host name for the connection.
    - mycommonname is the FDQN or common name that is used in the certificate.
```
{: screen}

### 發生的原因
{: #not-in-cn-why-it-is-happening}
內部部署應用程式與您針對此目的地而上傳至 {{site.data.keyword.Bluemix_notm}} 之憑證中的「通用名稱」（例如，伺服器 FQDN 或 YOUR 名稱）不相符。

- 請檢查下列項目：
- 您已正確地產生憑證，且使用正確的伺服器 FQDN 或主機名稱。
- 您已針對此用戶端將正確憑證上傳至 {{site.data.keyword.Bluemix_notm}} 目的地。

### 修正方式
{: #not-in-cn-how-to-fix-it}

 1. 在 {{site.data.keyword.Bluemix_notm}} 使用者介面中，移至「Secure Gateway 儀表板」。
 2. 選取目的地，然後按一下「編輯」圖示。
 3. 按一下「上傳憑證」。
 4. 上傳要用來連接至內部部署系統的 PEM 憑證檔案。

## IP 不在憑證的清單中
{: #san}

### 發生的狀況
{: #san-what-is-happening}
所提出之憑證中的 CN 是閘道的 IP 位址，但憑證沒有與 IP 位址相符的 SAN，因此用戶端無法連接。  

基於主機名稱解析問題，我們在目的地中使用 IP 位址。所提出之憑證中的 CN 是閘道的 IP 位址，但憑證沒有與 IP 位址相符的 SAN，因此用戶端無法連接。

您已建立使用 TLS 的目的地，但卻使用其 IP 位址，而非使用目的地的主機名稱。連接用戶端時，會擲出下列錯誤。

```
[2015-10-15 13:00:04.866] [INFO] Connection #0 is being established to 10.3.20.31:443
[2015-10-15 13:00:05.426] [ERROR] Connection #0 to destination 10.3.20.31:443 had error: IP: 10.3.20.31 is not in the cert's list:
[2015-10-15 13:00:05.427] [INFO] Connection #0 to 10.3.20.31:443 was closed
```
{: screen}

### 發生的原因
{: #san-why-it-is-happening}
發生的狀況是閘道用戶端中的 SSL 驗證碼會以不同的方式處理此目的地，因為它使用 IP 位址，而非主機名稱。它會在憑證的 SAN 中尋找 IP 位址的相符項，而非使用憑證的 CN 進行比對。由於憑證中沒有 SAN，因此將它視為不正確的連線，並讓 SSL 信號交換失敗。

### 修正方式
{: #san-how-to-fix-it}
如果您查看錯誤訊息並不會看到 CN（例如 [ERROR] Connection ## had error: Host: . is not cert&apos;s CN:），但憑證的清單讓我相信您已不正確地產生自簽憑證。問題在於憑證是使用 FQDN 或 CN 搭配 IP 位址而產生，這不會有任何作用，因為只有在使用 SAN 時才支援 IP 位址。

使用 openssl 來產生以 IP 作為 CN 之憑證的方法：

1. 建立 openssl 配置檔（我已從 /usr/lib/ssl/openssl.cnf 複製自己的檔案）
2. 將 alternate_names 區段新增至檔案中，如下所示：

   ```
   [ alternate_names ]

   IP.1 = &lt;my application&apos;s ip&gt;
   ```
   {: codeblock}

3. 在 [v3_ca] 區段中，新增這一行：

    ```
    subjectAltName = @alternate_names
    ```
    {: pre}

4. 在 CA_default 區段下，解除 copy_extensions 的註解（延伸複製選項：使用時請小心）：

    ```
    copy_extensions = copy
    ```
    {: pre}

5. 產生私密金鑰

    ```
    openssl genrsa -out private.key 3072
    ```
    {: pre}

6. 使用組織選項產生憑證

    ```
    openssl req -new -x509 -key private.key -sha256 -out certificate.pem -days 730 -config
    ```
    {: pre}

7. 結合檔案

    ```
    cat private.key certificate.pem > SAN.pem
    ```
    {: pre}

8. 將 SAN.pem 檔案載入至您的目的地，以作為 client-TLS 憑證。
9. 將 SAN.pem 檔案載入至您的內部部署應用程式，並重新啟動。

10. 可以針對 TCP、HTTP 或 HTTPS 配置您的目的地，而且您的雲端應用程式現在應該可以連接至內部部署應用程式。

如果您發現 UNABLE_TO_VERIFY_LEAF_SIGNATURE 問題，請檢查 openssl.conf 檔案，並將下列內容：

    ```
    keyUsage = digitalSignature, keyEncipherment
    ```
    {: pre}

變更為預設值：

    ```
    keyUsage = nonRepudiation, digitalSignature, keyEncipherment
    ```
    {: pre}

## 連線錯誤訊息：DEPTH_ZERO_SELF_SIGNED_CERT
{: #depth-zero}

### 發生的狀況
{: #depth-zero-what-is-happening}
您嘗試使用 Secure Gateway 用戶端實作內部部署的用戶端 TLS，並且收到下列錯誤訊息。

```
[ERROR] Connection #<connection ID> had error: DEPTH_ZERO_SELF_SIGNED_CERT Where:

    connection ID is a client assigned connection number.
```
{: screen}

### 發生的原因
{: #depth-zero-why-it-is-happening}
所定義的目的地遺漏用戶端憑證。

### 修正方式
{: #depth-zero-how-to-fix-it}
 1. 在 {{site.data.keyword.Bluemix_notm}} 使用者介面中，移至「Secure Gateway 儀表板」。
 2. 選取目的地，然後按一下「編輯」圖示。
 3. 按一下「上傳憑證」。
 4. 上傳要用來連接至內部部署系統的 PEM 憑證檔案。


## 如何在 Docker 用戶端中以互動方式載入 ACL 檔案？
{: #docker-load-acl}

### 發生的狀況
{: #docker-load-acl-what-is-happening}
由於 Docker 是容器或虛擬化環境，因此在實際啟動容器之前，無法直接存取您的檔案系統。這會讓它無法讀取主機檔案系統，直到它實際啟動並執行為止。

### 修正方式
{: #docker-load-acl-how-to-fix-it}
您可以採取的動作如下：

- 建立 Dockerfile 以包含 aclfile.txt

```
FROM ibmcom/secure-gateway-client
ADD aclfile.txt /tmp/aclfile.txt
```
{: codeblock}

- 建置新的 Docker 映像檔

```
docker build -t ads-secure-gateway-client .
```
{: pre}

- 執行新的 Docker 映像檔（需要指定 -t 及 -i 選項，否則您會遇到錯誤：找不到檔案或 ENOENT）：

```
docker run -t -i ads-secure-gateway-client1  --F /tmp/aclfile.txt
```
{: pre}

- 您應該得到下列輸出：

```
[2015-09-30 16:50:32.084] [INFO] The current access control list is being reset and replaced by the user provided file: /tmp/aclfile.txt
[2015-09-30 16:50:32.086] [INFO] The ACL file process accepts acl allow :8000
[2015-09-30 16:50:32.087] [INFO] The ACL file process accepts acl deny local
```
{: screen}

## 取得其他說明及支援
{: #getting-help-and-support}

如果您有使用 Secure Gateway 開發或部署應用程式的相關技術問題，請將問題張貼在 [Stack Overflow ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](http://stackoverflow.com/search?q=securegateway+ibm-bluemix)。請使用 "ibm-bluemix" 和 "secure-gateway" 來標記問題，讓 {{site.data.keyword.Bluemix_notm}} 開發團隊可以更輕鬆地找到。

如果您有服務或如何開始使用之指示的相關問題，請使用 [IBM developerWorks dW Answers ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://developer.ibm.com/answers/topics/securegateway/?smartspace=bluemix) 討論區以及 "bluemix" 和 "Secure Gateway" 標籤。

如需有關使用這些討論區的詳細資料，請參閱[這裡的取得協助頁面](https://console.ng.bluemix.net/docs/support/index.html#getting-help)。

如需開立 IBM 支援問題單的相關資訊，或支援層次與問題單嚴重性的相關資訊，請參閱[與支援中心聯絡](https://console.ng.bluemix.net/docs/support/index.html#contacting-support)。

在提交問題單時，請盡可能提供下列資訊：

- 用戶端執行所在的 OS 為何？
- 使用的用戶端版本（這可以在用戶端上使用 'C' 指令來找到）為何？
- 如果這是使用者介面問題，則請貼上或附加任何相關聯的瀏覽器主控台日誌及擷取畫面
- 貼上或附加任何相關聯的要求端應用程式日誌及時區
- 貼上或附加任何相關聯的「Secure Gateway 用戶端」日誌及時區
- 提供所使用目的地詳細資料（擷取畫面或填寫下列欄位）：
   - 目的地 ID
   - 通訊協定
   - 目的地端的鑑別
   - 已上傳的憑證（只要名稱以及它們上傳到哪個 Box 資料夾或鏈結）
