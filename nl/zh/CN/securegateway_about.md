---

copyright:
  years: 2015, 2017
lastupdated: "2017-04-10"

---

# 关于 {{site.data.keyword.SecureGateway}}
{: #about-sg}

## 它的安全程度如何？
{{site.data.keyword.SecureGatewayfull}} 在 {{site.data.keyword.SecureGateway}} 客户机（在内部部署网络中）和 {{site.data.keyword.SecureGateway}} 服务器之间维护一个持久加密 (TLS V1.2) 连接。通过此双向连接，可以安全地在云资源和内部部署资源之间传输数据。对于任一端上的连接（云资源与 SG 服务器之间的连接以及内部部署资源与 SG 客户机之间的连接），用户可定义对其实施的协议（TCP、TLS、HTTP、HTTPS 或 UDP）和安全措施（相互认证或 iptables）。  

## 双向支持
通过将传统的内部部署目标与新的云目标组合在一起，实现了完整的双向支持。新的云目标支持内部部署服务/应用程序通过 {{site.data.keyword.SecureGateway}} 客户机向云应用程序发送信息/请求。创建新的云目标时，目标将包含`资源主机`（云应用程序的主机名或 IP）、`资源端口`（云应用程序将侦听的端口）和`客户机端口`（{{site.data.keyword.SecureGateway}} 客户机将侦听的端口）。建立此目标后，内部部署服务即可连接到客户机端口并开始向 {{site.data.keyword.SecureGateway}} 服务器发送数据（这些数据将通过客户机进行发送），然后发送到指定的云应用程序。

为目标提供的客户机端口在关联的网关中必须唯一。例如，一个网关不能有两个逆向目标在端口 12000 上进行侦听，但两个网关可以各自有一个目标在端口 12000 上进行侦听。不过，在后一种情况中，如果这两个网关都连接到同一客户机，那么将只有其中一个目标能够在端口 12000 上进行侦听。

### 云目标
![云目标](./images/reverseDestination.png?raw=true "云目标")

### 内部部署目标
![内部部署目标](./images/onPremDestination.png?raw=true "内部部署目标")

## 高可用性
为了获得高可用性，请创建多个与相同网关标识相连的客户机连接。这可以通过将多个单网关客户机连接到同一网关标识和/或通过在一个多网关客户机上向同一网关标识发出多个连接来完成。连接将按循环法在所有连接的客户机之间分发。
