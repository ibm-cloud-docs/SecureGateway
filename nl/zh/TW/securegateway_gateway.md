---

copyright:
  years: 2015, 2017
lastupdated: "2017-04-10"

---

# 新增閘道
{: #add-sg-gw}

閘道可以視為識別特定網路或環境的方式。「Secure Gateway 用戶端」將用它來建立與「Secure Gateway 伺服器」的連線，而且它可以包含多個資源定義或目的地。

![Secure Gateway 儀表板](./images/newDashboard.png?raw=true "Secure Gateway 儀表板")

從「Secure Gateway 儀表板」，按一下「新增閘道」按鈕以開啟「新增閘道」畫面。

![新增閘道](./images/addGateway.png?raw=true "新增閘道")

此畫面上的唯一必要輸入是您的「閘道名稱」。依預設，會選取`需要安全記號`及`記號有效期限`。

透過要求安全記號以連接用戶端，每次啟動 Secure Gateway 用戶端時，都必須同時提供「閘道 ID」及「安全記號」。如果您取消勾選`需要安全記號`方框，則只需要在開始對閘道的連線時提供「閘道 ID」給用戶端。

「安全記號」的預設到期日是從建立時算起的 90 天。若要變更到期日，請維持勾選`記號有效期限`方框，然後以您希望記號到期的天數來編輯文字欄位（下限為 1，上限為 365）。若要建立不會到期的記號，請取消勾選`記號有效期限`方框。  

若要完成閘道的建立，請按一下「新增閘道」。建立閘道之後，此頁面會自動導覽至您的新閘道。

![新建閘道](./images/newGateway.png?raw=true "新建閘道")

既然您擁有新建立的閘道，便可以[連接第一個用戶端](/docs/services/SecureGateway?topic=securegateway-add-client)。
