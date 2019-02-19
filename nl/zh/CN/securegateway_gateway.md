---

copyright:
  years: 2015, 2017
lastupdated: "2017-04-10"

---

# 添加网关
{: #add-sg-gw}

网关可视为识别特定网络或环境的方法。Secure Gateway 客户机将使用网关来建立与 Secure Gateway 服务器的连接，并且可以包含多个资源定义或目标。

![Secure Gateway 仪表板](./images/newDashboard.png?raw=true "Secure Gateway 仪表板")

在 Secure Gateway 仪表板中，单击“添加网关”按钮以打开“添加网关”面板。

![添加网关](./images/addGateway.png?raw=true "添加网关")

此面板上唯一需要输入的是“网关名称”。缺省情况下，`需要安全性令牌`和`令牌到期时间`已选中。

通过需要安全性令牌来连接客户机，每次启动 Secure Gateway 客户机时，都必须同时提供网关标识和安全性令牌。如果取消选中`需要安全性令牌`框，那么只需提供客户机的网关标识就能进行连接。

安全性令牌的缺省到期日期为创建该令牌后 90 天。要更改到期日期，请保持选中`令牌到期时间`框，然后编辑相应文本字段并填写您希望令牌在多少天后到期（最小值为 1 天，最大值为 365 天）。要创建不会到期的令牌，请取消选中`令牌到期时间`框。  

要完成网关的创建，请单击“添加网关”。创建了网关后，该页面会自动导航至新网关。

![新网关](./images/newGateway.png?raw=true "新网关")

既然您已新创建了网关，那么现在可以[连接第一个客户机](/docs/services/SecureGateway/securegateway_client.html)。
