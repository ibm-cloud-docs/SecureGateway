---

copyright:
  years: 2015, 2018
lastupdated: "2018-08-10"

---

# {{site.data.keyword.SecureGateway}} 服务套餐
{: #secure-gateway-service-plans}

在 V1.7.0 发行版中，引入了新的分层套餐定价模型，支持 {{site.data.keyword.SecureGateway}} 按任何公司的需求进行扩展。下表提供了与每个公开可用套餐关联的限制：

![分层套餐模型](./images/planDetails.png?raw=true "分层套餐模型")

## 更改套餐
更改套餐时，新套餐会被视为升级或降级。这由每个套餐所允许的缺省网关数来确定（例如，从 Professional (5) 更改为 Enterprise (25) 是升级）。升级时，不会中断服务；但是，降级会将所有网关更新为[不活动](/docs/services/SecureGateway/securegateway_faq.html#states)，这需要您在复原服务之前重新激活网关和目标（不超过新的套餐限制）。

<b>注</b>：从标准套餐转换为任何新套餐时，都会被视为降级。


## 有关超过限制的通知
创建的网关数/目标数达到允许的最大值时，网关/目标的仪表板中会显示警告日志。

达到客户机数量时，如果有新的客户机尝试连接到网关，那么客户机日志中将显示错误日志，并且网关会使新客户机退出。

数据使用量超过数据传输限额时，客户机日志中将显示错误日志，并且网关会使客户机退出。
