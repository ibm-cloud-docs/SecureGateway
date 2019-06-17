---

copyright:
  years: 2015, 2019
lastupdated: "2019-06-17"

---

# Adding a Gateway
{: #add-sg-gw}

A gateway can be thought of as a way to identify a particular network or environment.  It is what a Secure Gateway Client will use to establish connectivity with the Secure Gateway servers and can contain multiple resource definitions, or destinations.

![Secure Gateway Dashboard](./images/newDashboard.png?raw=true "Secure Gateway Dashboard")

From the Secure Gateway Dashboard, click the Add Gateway button to open the Add Gateway panel.

![Add Gateway](./images/addGateway.png?raw=true "Add Gateway")

The only required input on this panel is your Gateway Name.  By default, both `Require security token` and `Token Expiration` are selected.

By requiring the security token to connect clients, each time you start a Secure Gateway client, you will have to provide both the Gateway ID and the Security Token.  If you uncheck the `Require security token` box, you will only have to provide the Gateway ID to the client when starting the connection to the gateway.

The default expiration date for the Security Token is 90 days from when it is created.  To change the expiration date, keep the `Token Expiration` box checked and edit the text field with the number of days you would like the token to expire in (minimum 1, maximum 365).  To create a token that does not expire, uncheck the `Token Expiration` box.  

To finish the creation of your gateway, click Add Gateway.  Once the gateway is created, the page will automatically navigate into your new gateway.

![New Gateway](./images/newGateway.png?raw=true "New Gateway")

Now that you have your newly created gateway, you can [connect your first client](/docs/services/SecureGateway?topic=securegateway-add-client).

## Regenerate security token
{: #regen-sectoken}

By clicking the Settings button on any of the gateway tiles, you can edit the configuration of the gateway and check the `Current Token Expiring` on the panel.

![Guided Setup](./images/editGateway.png?raw=true "Edit Gateway")

When the security token is expired, it will not affect the connecting Secure Gateway Client, since the security token is only used when start/restart a Secure Gateway Client.

There is no way of extending the expiration of the existing security token, when the security token is expired, you will need to regenerate it, once the Secure Gateway Client restart, it will need to use the latest security token.

To regenerate the security token, you will need to click the `Regenerate Token` link. The `Token Expiration` will only take effect when you click the `Regenerate Token` link, but not the `Save` button. Please check the `Current Token Expiring` after you regenerate the security token.
