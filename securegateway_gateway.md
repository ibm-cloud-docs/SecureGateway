---

copyright:
  years: 2015, 2021
lastupdated: "2021-03-17"

subcollection: SecureGateway

---
{:deprecated: .deprecated}
{:external: target="_blank" .external}

# Adding a Gateway
{: #add-sg-gw}

Secure Gateway is being deprecated. For more information, see [https://ibm.biz/securegateway-deprecation](https://ibm.biz/securegateway-deprecation){: external}.
{: deprecated}

A gateway can be thought of as a way to identify a particular network or environment.  It is what a Secure Gateway Client will use to establish connectivity with the Secure Gateway servers and can contain multiple resource definitions, or destinations.

![Secure Gateway Dashboard](./images/newDashboard.png?raw=true "Secure Gateway Dashboard")

From the Secure Gateway Dashboard, click the Add Gateway button to open the Add Gateway panel.

![Add Gateway](./images/addGateway.png?raw=true "Add Gateway")

The only required input on this panel is your Gateway Name.  By default, both `Require security token` and `Token Expiration` are selected.

By requiring the security token to connect clients, each time you start a Secure Gateway client, you will have to provide both the Gateway ID and the Security Token.  If you uncheck the `Require security token` box, you will only have to provide the Gateway ID to the client when starting the connection to the gateway.

The default expiration date for the Security Token is 90 days from when it is created.  To change the expiration date, keep the `Token Expiration` box checked and edit the text field with the number of days you would like the token to expire in (minimum 1, maximum 365).  To create a token that does not expire, uncheck the `Token Expiration` box.  

To finish the creation of your gateway, click Add Gateway.  Once the gateway is created, the page will automatically navigate into your new gateway.

![New Gateway](./images/newGateway.png?raw=true "New Gateway")

Now that you have your newly created gateway, you can [connect your first client](/docs/services/SecureGateway?topic=SecureGateway-add-client).

## Regenerate security token
{: #regen-sectoken}

By clicking the Settings button ![Setting Button](./images/settingIcon.png?raw=true "Setting Button") on any of the gateway tiles, you can edit the configuration of the gateway and check the `Current Token Expiring` on the panel.

![Guided Setup](./images/editGateway.png?raw=true "Edit Gateway")

When the security token is expired, it will not affect the connecting Secure Gateway Client, since the security token is only used when start/restart a Secure Gateway Client. However, you will not able to configure the destinations until you regenerate the security token.

There is no way of extending the expiration of the existing security token, when the security token is expired, you will need to regenerate it, once the Secure Gateway Client restart, it will need to use the latest security token.

To regenerate the security token, you will need to click the `Regenerate Token` link. The `Token Expiration` will only take effect when you click the `Regenerate Token` link, but not the `Save` button. Please check the `Current Token Expiring` after you regenerate the security token.

## Reactivate gateway
{: #reactivate}

![Manage Activity](./images/manageActivity.png?raw=true "Manage Activity")

When you [downgrade the plan](/docs/SecureGateway?topic=SecureGateway-secure-gateway-service-plans#changing-plans), it will update all gateways to be [inactive](/docs/SecureGateway?topic=SecureGateway-sg-faq#states-answer-non-functional), all provisioned cloud port of the destinations will be reset. By clicking the wrench button ![Wrench Button](./images/wrenchIcon.png?raw=true "Wrench Button") in the gateway panel, you can reactivate your gateways with the arrow button ![Arrow Button](./images/arrowIcon.png?raw=true "Arrow Button"), and the configuration will be applied after you click the `Apply` button. After you reactivate your gateways, you can [reactivate the destinations](/docs/SecureGateway?topic=SecureGateway-add-dest#reactivate) within the gateway in the destination panel.

## Import/Export gateway
{: #import}

If you want to copy the gateway/destination configuration to another Secure Gateway instance, you can click the Export button ![Export Button](./images/exportIcon.png?raw=true "Export Button") on the gateway tile to export the configuration of the gateway/destination in the gateway panel of the original Secure Gateway instance, then click the Import button ![Import Button](./images/importIcon.png?raw=true "Import Button") to import the configuration in the gateway panel of the target Secure Gateway instance. The imported gateway will obtain another security token and might be created on another Node, and the imported On-premises destinations will obtain another cloud host and port.
