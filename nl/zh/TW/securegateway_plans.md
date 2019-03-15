---

copyright:
  years: 2015, 2018
lastupdated: "2018-08-10"

---

# {{site.data.keyword.SecureGateway}} 服務方案
{: #secure-gateway-service-plans}

隨著 1.7.0 版的發行，已引進新的分層方案定價模型，以容許 {{site.data.keyword.SecureGateway}} 根據任何公司的需求而調整。下表提供與每個公開可用方案相關聯的限制：

![分層方案模型](./images/planDetails.png?raw=true "分層方案模型")

## 變更方案
{: #changing-plans}
當您變更方案時，會將您的新方案視為升級或降級。這是由每個方案容許的預設閘道數目決定（例如 Professional (5) 到 Enterprise (25) 是升級）。升級時，服務將不會岔斷；不過，降級會將所有閘道都更新為[非作用中](/docs/services/SecureGateway/securegateway_faq.html#faq-states)，而這會需要您先重新啟動 (reactivate) 閘道及目的地（最多到新方案限制），然後才能還原服務。

<b>附註</b>：從「標準」方案轉移至任何新方案時，會將這視為降級。


## 已超出限制的通知
{: #limit-exceeded-notifications}
當您建立容許的閘道/目的地數量上限時，會在閘道/目的地的儀表板中顯示警告層次日誌。

如果達到用戶端數量，而且有新的用戶端嘗試連接至閘道，則將會在用戶端日誌中顯示錯誤層次日誌，而且閘道會結束新的用戶端。

資料用量超出資料傳送額度時，將會在用戶端日誌中顯示錯誤層次日誌，而且閘道會結束用戶端。
