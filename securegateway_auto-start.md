---

copyright:
  years: 2015, 2021
lastupdated: "2021-03-17"

subcollection: SecureGateway

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:deprecated: .deprecated}
{:external: target="_blank" .external}

# Configuring Auto-start for the Client
{: #auto-start-conf}

Secure Gateway is being deprecated. For more information, see [https://ibm.biz/securegateway-deprecation](https://ibm.biz/securegateway-deprecation){: external}.
{: deprecated}

## Linux
{: #auto-start-linux}

If you have chosen to use you system's auto-start facility, use one of the methods below to start the client.  The configuration file that will be used by the client can be found at:

```
/etc/ibm/sgenvironment.conf
```
{: screen}

This file includes the following important variables to set:

| Environment Variable | Description       |
| ------------- | ----------- |
| RESTART_CLIENT | Stop and Restart the client during install or upgrade (Yes or No) |
| GATEWAY_ID | Your gateway ID as created on the Secure Gateway for IBM Cloud UI |
| SECTOKEN | Security Token for this Gateway ID, if you choose to enforce security when creating the gateway |
| ACL_FILE | Access Control List File you want to use to restrict on-premises access to resources |
| LOGLEVEL | The log level you want to set for your service (default is INFO) |
| USE_UI   | Set this to 'N' if you don't want to launch the client UI |
| UI_PORT  | The port on which you want to launch client UI on (default is 9003) |
| LANGUAGE | The language you want to have client logs in (default is en) |
| SECGW_ARGS | The addtional [startup options](/docs/services/SecureGateway?topic=SecureGateway-client-interacting#startup-args) you want to add (default is `--no_license --l $LOGLEVEL --service`) |

<b>Note:</b> This file is only read if you are using your system's auto-start facility.  If you are running the client manually this file is ignored.

### Upstart
{: #auto-start-upstart}

If you using the upstart capability so that the Secure Gateway client runs automatically at system startup, you have to check its configuration first (/etc/ibm/sgenvironment.conf).  Once you have configured the upstart service you can use the following command to start it:

```
sudo initctl start securegateway_client
```
{: pre}

To restart the service:

```
sudo initctl restart securegateway_client
```
{: pre}

To stop the service, run the following command:

```
sudo initctl stop securegateway_client
```
{: pre}

### SystemD
{: #auto-start-systemd}

If you are using the systemD capability so that the Secure Gateway client runs automatically at system startup, you have to check its configuration first (/etc/ibm/sgenvironment.conf).  Once you have configured the upstart service you can use the following command to start it:

```
systemctl start securegateway_client
```
{: pre}

To restart the service:

```
systemctl restart securegateway_client
```
{: pre}

For status on the service:

```
systemctl status securegateway_client
```
{: pre}

To stop the service, run the following command:

```
systemctl stop securegateway_client
```
{: pre}

### System V
{: #auto-start-system-v}

System V is not setup during installation like the other auto-start facilities. Scripts are available in the installation directory /opt/ibm/securegateway/client/upstart, they are:

```
secgw.sh
securegateway_clientd
```
{: screen}

<b>Note:</b> This section will affect users of SuSE/SLES 11.

Optional: If you are running SuSE version 11 that uses System V auto-start, scripts are provided that can be used to configure this process. Follow the below procedure:

```
cd /opt/ibm/securegateway/client/upstart/systemV
cp secgw.sh /usr/local/bin
chmod +x /usr/local/bin/secgw.sh
cp securegateway_clientd /usr/local/bin
chmod +x /usr/local/bin/securegateway_clientd
cd /etc/init.d
ln -s /usr/local/bin/securegateway_clientd
insserv securegateway_clientd
vi /etc/ibm/sgenvironment.conf
```
{: codeblock}

Once these steps have been executed, the YasT and System V commands can be used to start/stop the daemon.

### AIX
{: #auto-start-aix}

When installing the Secure Gateway client on AIX system, the `forever` will be installed as well, it can help you to start Secure Gateway client as a background process, you can use `forever --version` to check.

To start the Secure Gateway client as a background process, you have to check its configuration first (/etc/ibm/sgenvironment.conf).  Once you have configured you can use the following script to start it:

```
/opt/ibm/securegateway/client/start_sgclient
```
{: pre}

To restart the service:

```
forever restart sgclient
```
{: pre}

To stop the service, run the following command:

```
forever stop sgclient
```
{: pre}

Please use the `start_sgclient` script to start the service again when it is stopped.

**Limitation: You must run the `start_sgclient` script after each reboot.**

Return to [Getting Started - Adding a Client](/docs/services/SecureGateway?topic=SecureGateway-add-client).

## Windows
{: #auto-start-windows}

To alter the state of the windows service, open a command window with administrator privileges.

```
cd "<Installation_directory>\ibm\securegateway\client"
```
{: pre}

If the service is already running, the user can use the below command to stop it

```
windowsService.cmd uninstall
```
{: pre}

To start the service, use the following command

```
windowsService.cmd install
```
{: pre}

<b>Note:</b> The service will run based on the configurations stored in:

```
%Installation_directory%/ibm/securegateway/client/securegw_service.config
```
{: pre}

The application logs for windows service will be stored at:

```
%Installation_directory%/ibm/securegateway/client/logs/securegw_win_service.log
```
{:pre}

 The logs are rolled into a new file on a daily basis.

Return to [Getting Started - Adding a Client](/docs/services/SecureGateway?topic=SecureGateway-add-client).
