---

copyright: 
  years: 2023, 2023
lastupdated: "2023-10-25"

keywords: secure gateway, deprecation, migration

subcollection: SecureGateway

---

{{site.data.keyword.attribute-definition-list}}


# Migration options
{: #dep-migration-options}

{{site.data.keyword.SecureGateway}} is deprecated. For more information, see the [deprecation details](/docs/SecureGateway?topic=SecureGateway-dep-overview).
{: deprecated}

Review the following options for migrating your secure data communications from {{site.data.keyword.SecureGateway}} to another {{site.data.keyword.cloud_notm}} service.
{: shortdesc}

## {{site.data.keyword.satellitelong}}
{: #migration_options_satellite}

{{site.data.keyword.satelliteshort}} employs the most current encryption standards for a mature and fully capable distributed hybrid cloud solution. {{site.data.keyword.satelliteshort}} Connector provides hybrid cloud communications with a lightweight container-based deployment model, which extends access to cloud resources and operational simplicity to all sites. 

{{site.data.keyword.satelliteshort}} allows you to launch consistent cloud services across any cloud, on-premises, and at the edge with speed and simplicity. It includes {{site.data.keyword.satelliteshort}} Link functionality, which improves upon the Secure Gateway client experience with a highly available and secure-by-default communication between the cloud and client premise, third-party clouds or network edge.

- **Consistent public cloud experience across geographies and clouds**: {{site.data.keyword.satelliteshort}} provides a consistent cloud experience in all parts of the world, regardless of the lack of any given public cloud provider’s regional presence. A consistent development platform results in higher development velocity.
- **Co-locating data and processing**: {{site.data.keyword.satelliteshort}} allows data processing to happen close to the data, alleviating latency. This is especially important when using predictive AI analytics or other critical applications with large date sets.
- **Data sovereignty, security and compliance needs**: {{site.data.keyword.satelliteshort}} allows client data to remain in-country, meeting local regulatory, contractual, information security or compliance needs. 

{{site.data.keyword.satelliteshort}} now has two deployment models to best fit a your needs. 

{{site.data.keyword.satelliteshort}} Location
:   Locations use the your x86 host resources to create a highly available availability zone on the client’s premise, supporting both {{site.data.keyword.satelliteshort}} Link’s communication functionality and the ability to deploying managed cloud services on premise, such as managed OpenShift, managed databases, and more.

{{site.data.keyword.satelliteshort}} Connector
:   A new deployment model which enables only the secure communications with cloud via a light weight container deployed by the client on their own docker hosts. This option brings all the security and auditability of {{site.data.keyword.satelliteshort}} communication, but with much lower resources required. Connector provides the same application-level transport through common ports as Secure Gateway, with greater client visibility and audit control. It’s light-weight container based deployment is focused for clients that just need the hybrid cloud communication features without the need for additional on-premise cloud services. Clients can also choose to utilize {{site.data.keyword.satelliteshort}} Locations instead, which provide the same communications services while also enabling the deployment of IBM managed services and features on-premises, at the edge or within other cloud providers. 


Both {{site.data.keyword.satelliteshort}} Locations and {{site.data.keyword.satelliteshort}} Connectors are located in a given IBM Cloud Multi Zone Region (MZR). Both provide application-layer transport (Layer 4) between IBM services or their own applications running within IBM Cloud and the resources running at the client's location. A Satellite Location goes further, allowing clients to run cloud applications locally, wherever the client needs, to address a number of real-world challenges, as well as accessing cloud hosted applications and services via an encrypted and secured channel.

To learn more about {{site.data.keyword.satelliteshort}} Connector, complete the [Reviewing Satellite Connector as a Secure Gateway replacement](/docs/SecureGateway?topic=SecureGateway-understanding-connector) tutorial.


## Virtual Private Network
{: migration_options_vpn}

Another option to replace Secure Gateway is a Virtual Private Network (VPN), Secure Gateway is a type of specialized VPN. In essence, VPNs enable users to share data across public networks as though they were using a private network. They create virtual point-to-point connections using tunneling protocols, encryption and dedicated connections, which facilitate secure and functional environments for the data to be shared.

There are a few VPN-related technologies on the IBM Cloud that provide this level of VPN connection capability:

IPSec VPN
:   VPN facilitates connectivity from your secure network to IBM IaaS platform’s private network. A VPN connection from your location to the private network allows for out-of-band management and server rescue through an encrypted VPN tunnel. Communicating using the private network is inherently more secure and gives users the flexibility to limit public access while still being able to access their servers. Any user on your account can be given VPN access, which is available as both SSL and PPTP.

VPN for VPC
:   With Virtual Private Cloud (VPC), you can quickly provision generation 2 virtual server instances for VPC with high network performance. VPC infrastructure contains a number of Infrastructure-as-a-Service (IaaS) offerings, including Virtual Servers for VPC.

## Comparing scenarios for VPN and IBM Cloud {{site.data.keyword.satelliteshort}} Connector
{: #migration_options_usage}

The products mentioned earlier are useful in different circumstances and bring different levels of capability to your Cloud experience. IBM Cloud {{site.data.keyword.satelliteshort}} is considered a distributed cloud, which is also what parts of Secure Gateway could generally be described as. So, when do you want a distributed cloud, and when do you want a VPN?

One of the key differences between VPNs and distributed clouds is that VPNs expose the entire network by default. This is useful if the intention is to share significant amounts of resources over the network, but it requires extensive configuration to secure the resources that you don’t want to share. 

Distributed clouds approach things in the opposite manner which means access to local resources is denied by default. To allow access to a resource, it has to be added as a "location," and authorization needs to be granted in the access control list. This makes IBM Cloud {{site.data.keyword.satelliteshort}} a powerful choice if there is a limited amount of resources that need to be accessed, because it involves significantly less configuration to keep other assets secure.

You can use both distributed clouds and VPNs to connect and access resources through the IBM Cloud, but the ideal option will depend on your use case. If only a limited set of resources needs to be accessed, then a distributed cloud is probably the best way to go. This is because IBM Cloud {{site.data.keyword.satelliteshort}} is quicker and easier to configure and won’t run the risk of exposing assets that were intended to remain closed off.

If you need to share a lot of resources between various offices, VPNs can be a better choice. The administrators can then configure the VPN to lock down those resources that they don’t want shared over the virtual network.

So, the decision of what kind of technology to use when securing your cloud resources depends on your intended usage, overall security concerns, and level of configuration effort required with each method. However, if you want the very latest in computing capability, flexibility in infrastructure options, a consistent operational experience, and the very best security possible across multiple locations, you should use {{site.data.keyword.satelliteshort}}.

## Next steps
{: #migration_options_next}

To learn more about {{site.data.keyword.satelliteshort}} Connector, complete the [Reviewing Satellite Connector as a Secure Gateway replacement](/docs/SecureGateway?topic=SecureGateway-understanding-connector) tutorial.

## Additional resources
{: #migration_options_additional_resources}

Check out the following links for more information on IBM Cloud {{site.data.keyword.satelliteshort}} and {{site.data.keyword.satelliteshort}} Connector, or contact your IBM sales representative.

- [{{site.data.keyword.satelliteshort}} overview](https://www.ibm.com/products/satellite){: external}
- [{{site.data.keyword.satelliteshort}} Connector overview](/docs/satellite?topic=satellite-understand-connectors)
- [Build Faster, Securely, Anywhere with IBM Cloud {{site.data.keyword.satelliteshort}}](https://www.ibm.com/blog/deploy-cloud-services-anywhere-with-ibm-cloud-satellite/){: external}
- [{{site.data.keyword.satelliteshort}} product demos on Framer](https://framer.com/share/External-{{site.data.keyword.satelliteshort}}-Demo--4QmxdNMF6smthRhzQgzt/ddN6j0Nrt?fullscreen=1&highlights=0#ddN6j0Nrt){: external}

