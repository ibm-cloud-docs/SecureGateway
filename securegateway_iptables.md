---

copyright:
  years: 2015, 2018
lastupdated: "2018-08-10"

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# IP Table Rules

To allow the enforcement of iptables rules on your destination, you must have the `Restrict network access` option checked under the Network Security panel of your destination.  At that point you can add the rules you want enforced, such as: 192.0.0.1 9000 (single IP and port),  192.0.0.1-192.0.0.5 5000:5005 (range of IPs and range of ports), or any combination of these rules. Please see [Configuring Network Security](./securegateway_destination.html#configuring-network-security) for more information.

If you are creating your private destinations with cURL, you could use a command like:

```
curl "https://sgmanager.ng.bluemix.net/v1/sgconfig/<gateway_id>/destinations" -H "Authorization: Bearer <security_token>" -H "Content-type: application/json" -d '{"desc":"My Private Destination","ip":"1.1.1.1","port":8000,"private":true}'
```
{: pre}

Once your private destination is created, you can add IP table rules with commands like:

```
curl -X PUT "https://sgmanager.ng.bluemix.net/v1/sgconfig/<gateway_id>/destinations/<destination_id>/ipTableRule" -H "Authorization: Bearer <security_token>" -H "Content-type: application/json" -d '{"src":"192.0.0.1","spt":"9000"}' -k
```
{: pre}

and

```
curl -X PUT "https://sgmanager.ng.bluemix.net/v1/sgconfig/<gateway_id>/destinations/<destination_id>/ipTableRule" -H "Authorization: Bearer <security_token>" -H "Content-type: application/json" -d '{"src_range":"192.0.0.1-192.0.0.5","spt":"5000:5005"}' -k
```
{: pre}

Please note that the first command uses `src` to provide a single IP whereas the second uses `src_range` to provide a range of IPs.

## IP Table Rules for Dynamic IPs

If your application has a dynamic set of IPs, but you do not know them you can leverage the {{site.data.keyword.SecureGateway}} 
REST API to update the ip table rules on the fly.

As an example, this short NodeJS program will update IP table rules for a Cloud Foundry application that runs multiple instances.

```javascript
const request = require('request')

//These are best configured using environment variables.
const GATEWAY_ID = 'XXXXXX'//Your Gateway ID
const DEST_ID = 'YYYYYY' // The Destination ID to restrict access to.
const SEC_TOKEN = 'ZZZZZ' // The Security Token for the Gateway.

const APP_ID = JSON.parse(process.env.VCAP_APPLICATION).application_id
const IP_TABLE_BODY = {
  app: APP_ID + ':' + process.env.CF_INSTANCE_INDEX //uniquely identifies the app and instance for ip table rule.
  src: process.env.CF_INSTANCE_IP 
 }
 
request({
  method: 'PUT',
  uri: `https://sgmanager.ng.bluemix.net/v1/sgconfig/$GATEWAY_ID/destinations/$DEST_ID/ipTableRule`
  headers: {
    'Authorization': `Bearer $SEC_TOKEN`
  }
  json: true, // Content-Type: application/json
  body: IP_TABLE_BODY
  }, console.log.bind(console)) 
```

This should be run when starting up the application, any IP tables rule which is defined before will be overwritten. Each IP table rule
uniquely identifies each instance of the application using the `application_id` and `CF_INSTANCE_INDEX`. The external IP address
is retrieved from the `CF_INSTANCE_IP` variable and applied to the IP table rule.


{: pre}
