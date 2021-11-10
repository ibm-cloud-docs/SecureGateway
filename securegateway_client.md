---

copyright:
  years: 2015, 2021
lastupdated: "2021-03-17"

subcollection: SecureGateway

---
{:deprecated: .deprecated}
{:external: target="_blank" .external}

# Adding a Client
{: #add-client}

The {{site.data.keyword.SecureGateway}} Client is the piece of the puzzle that makes the magic happen.  The client establishes the initial connection between the on-premises network and a gateway on the {{site.data.keyword.SecureGateway}} servers and allows for communication to pass through to the defined destinations.

![New Gateway](./images/newGateway.png?raw=true "New Gateway")

From within your new gateway and on the Clients tab, click the Connect Client button to open the Connect Client panel.

![Connect Client](./images/connectClient.png?raw=true "Connect Client")

After selecting your preferred client, [click here to see how to install it](/docs/services/SecureGateway?topic=SecureGateway-client-install).

Once you have installed your client, you can configure it to [start automatically](/docs/services/SecureGateway?topic=SecureGateway-auto-start-conf) or to [run interactively](/docs/services/SecureGateway?topic=SecureGateway-client-interacting).

![Connected Client](./images/connectedClient.png?raw=true "Connected Client")

If you would like to configure the Access Control List for your client now, [click here for more details](/docs/services/SecureGateway?topic=SecureGateway-acl).

Now that you have your client connected, you can [create your first destination](/docs/services/SecureGateway?topic=SecureGateway-add-dest).
