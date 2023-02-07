---

copyright:
  years: 2015, 2023
lastupdated: "2023-02-07"

subcollection: SecureGateway

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:deprecated: .deprecated}
{:external: target="_blank" .external}

# IP Table Rules
{: #iptables-rules}

To allow the enforcement of iptables rules on your destination, you must have the `Restrict network access` option checked under the Network Security panel of your destination.  At that point you can add the rules you want enforced, such as: 192.0.0.1 9000 (single IP and port),  192.0.0.1-192.0.0.5 5000:5005 (range of IPs and range of ports), or any combination of these rules. Please see [Configuring Network Security](/docs/services/SecureGateway?topic=SecureGateway-add-dest#dest-network-security) for more information.

If you are creating your private destinations with cURL, you could use a command like:

```
curl "https://sgmanager.us-south.securegateway.cloud.ibm.com/v1/sgconfig/<gateway_id>/destinations" -H "Authorization: Bearer <security_token>" -H "Content-type: application/json" -d '{"desc":"My Private Destination","ip":"1.1.1.1","port":8000,"private":true}'
```
{: pre}

Once your private destination is created, you can add IP table rules with commands like:

```
curl -X PUT "https://sgmanager.us-south.securegateway.cloud.ibm.com/v1/sgconfig/<gateway_id>/destinations/<destination_id>/ipTableRule" -H "Authorization: Bearer <security_token>" -H "Content-type: application/json" -d '{"src":"192.0.0.1","spt":"9000"}' -k
```
{: pre}

and

```
curl -X PUT "https://sgmanager.us-south.securegateway.cloud.ibm.com/v1/sgconfig/<gateway_id>/destinations/<destination_id>/ipTableRule" -H "Authorization: Bearer <security_token>" -H "Content-type: application/json" -d '{"src_range":"192.0.0.1-192.0.0.5","spt":"5000:5005"}' -k
```
{: pre}

Please note that the first command uses `src` to provide a single IP whereas the second uses `src_range` to provide a range of IPs.


{: pre}
