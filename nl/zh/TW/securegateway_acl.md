---

copyright:
  years: 2015, 2017
lastupdated: "2017-04-10"

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 存取控制清單

{{site.data.keyword.SecureGateway}} 用戶端提供內嵌的「存取控制清單 (ACL)」支援。您可以修改用戶端的 ACL，以容許或限制（拒絕）對內部部署資源的存取。可以使用用戶端指令，以互動方式來執行此作業，或是藉由指定一個包含您要讓其生效之 ACL 的檔案來執行。

從 1.5.0 版開始，所有連接至相同閘道之用戶端的「存取控制清單」規則皆已同步。這表示您只需要從單一用戶端建立/更新 ACL，它就會在所有連接至該閘道的執行中用戶端之間共用。ACL 也會在各階段作業之間持續保存，如此連接新的用戶端時，也會套用相同的 ACL 規則。

## 存取控制清單指令
{: #commands}

支援的 ACL 指令如下：

```
acl allow [<hostname>]:[<port>]
acl deny [<hostname>]:[<port>]
no acl [<hostname>]:[<port>]
no acl
show acl
```
{: screen}

省略主機名稱或埠的表單即表示可使用所有主機名稱或埠。例如，以下是針對所有主機名稱容許埠 22 的 ACL 規則。

```
acl allow :22
```
{: pre}

以下是針對所有主機名稱容許所有埠的另一個 ACL 規則範例，它實質上會停用 ACL 支援。<b>不建議您這麼做。</b>

```
acl allow :
```
{: pre}

`show acl` 指令將顯示目前設定的 ACL，或提供有關整體設定的訊息。

回到[開始使用 - 新增用戶端](/docs/services/SecureGateway/securegateway_client.html)。

## 使用 ACL 的 HTTP/S 路徑控制
{: #routes}

從 1.6.0 版開始，HTTP/S 目的地也可以在 ACL 項目上強制執行特定路徑。這些的新增方式與一般 ACL 項目相同，但將路徑附加至規則尾端。例如，下列指令只容許遵循 /my/api 路徑的要求通過：

```
acl allow localhost:80/my/api
```
{: pre}

此規則生效時，將容許 `<cloud host>:<cloud port>/my/api*` 的要求通過。

只有在 `acl allow` 指令上支援路徑。

## 存取控制清單優先順序
{: #precedence}

提供 `acl allow :` 指令之後，如果進一步輸入 `acl allow` 指令，則 `ALL:ALL` 容許規則（來自 `acl allow :`）將會從清單移除，並假設您不再想要容許進行無限制的存取。提供 `acl deny :` 指令之後，如果輸入另一個 `acl deny` 指令，則 `ALL:ALL` 拒絕規則（來自 `acl deny :`）將會從清單移除，並假設您不再想要限制所有存取。如果在 CLI 中透過
`show acl` 指令列出現行 ACL 規則，則會有一個指示器顯示容許還是拒絕未列出的規則。

## 匯入 ACL 檔案
{: #import}

您可以提供檔名給 `acl file` 指令，檔案中包含用戶端在啟動時將讀取的受支援 ACL 指令。此檔案應該具有下列格式的指令：

```
acl allow [<hostname>]:[<port>]
acl deny [<hostname>]:[<port>]
no acl [<hostname>]:[<port>]
no acl
```
{: screen}

<b>附註：</b>沒有任何其他參數的 `no acl` 會「重設」ACL 表格，並將存取設為 DENY ALL。

您可以在[這裡](/docs/services/SecureGateway/securegateway_acl-file.html)找到範例 ACL 檔案。

回到[開始使用 - 新增用戶端](/docs/services/SecureGateway/securegateway_client.html)。

## 將 ACL 檔案複製到 {{site.data.keyword.SecureGateway}} Docker 用戶端
{: #docker}

{{site.data.keyword.SecureGateway}} Docker 用戶端實際上是在它自己的虛擬化容器中執行。因此，在容器內執行的處理程序（包括 {{site.data.keyword.SecureGateway}} 用戶端）無法直接存取管理機器的檔案系統。從 Docker Engine 1.8.0 版開始，不論容器在執行中還是已停止，您都可以使用 'docker cp' 指令，將主機上的現有檔案推送至容器。必須執行此作業，才能使用 {{site.data.keyword.SecureGateway}} 用戶端的 ACL FILE 互動式指令。

若要在 Docker 中使用互動式 'cp' 支援從主機推送到 Docker 實例，您必須使用 Docker 1.8.0。您可以使用 `docker --version` 來檢查這點。

完成此作業之後，您的版本應該顯示如下。建議您容許 Docker 以非 root 使用者身分執行，因此建議在將引擎升級為 1.8.0 或 1.8.2 之後執行該指令。

```
Client:
 Version:      1.8.2
 API version:  1.20
 Go version:   go1.4.2
 Git commit:   0a8c2e3
 Built:        Thu Sep 10 19:21:21 UTC 2015
 OS/Arch:      linux/amd64

Server:
 Version:      1.8.2
 API version:  1.20
 Go version:   go1.4.2
 Git commit:   0a8c2e3
 Built:        Thu Sep 10 19:21:21 UTC 2015
 OS/Arch:      linux/amd64
```
{: screen}

然後，若要將 acl 檔案清單推送至 Docker 映像檔，請遵循下列步驟：

- 執行 'docker ps' 指令，以尋找您的容器 ID

```
CONTAINER ID IMAGE                        COMMAND                CREATED        STATUS  PORTS NAMES
764aadce386b ibmcom/secure-gateway-client "node lib/secgwclient" 27 seconds ago Up 26 seconds condescending_nobel
```
{: screen}

- 使用容器 ID 或名稱，用 'docker cp' 指令來複製 acl.list：

```
docker cp 01_client.list 764aadce386b:/root/01_client.list
```
{: pre}

- 接下來，在 Docker 中執行的 {{site.data.keyword.SecureGateway}} 用戶端內：

```
cli F /root/01_client.list

 [2015-10-01 08:12:30.091] [INFO] The current access control list is being reset and replaced by the user provided file: /root/01_client.list
 [2015-10-01 08:12:30.093] [INFO] The ACL file process accepts acl allow 127.0.0.1:27017
 [2015-10-01 08:12:30.094] [INFO] The ACL file process accepts acl allow 127.0.0.1:22
```
{: screen}

- 顯示 ACL：

```
cli> S
 -------------------------------------------------------------------
           -- Secure Gateway Client Access Control List --          

  hostname                               port                  value
  127.0.0.1                             27017                  Allow
  127.0.0.1                                22                  Allow
 -------------------------------------------------------------------
```
{: screen}

回到[開始使用 - 新增用戶端](/docs/services/SecureGateway/securegateway_client.html)。
