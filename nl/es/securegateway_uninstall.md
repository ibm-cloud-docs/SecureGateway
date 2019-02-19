---

copyright:
  years: 2015, 2017
lastupdated: "2017-04-25"

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Desinstalación del cliente
{: #client-uninstall}

## Docker
{: #docker}

Para eliminar la imagen Docker del cliente de {{site.data.keyword.SecureGateway}}, ejecute el siguiente mandato:

```
docker rmi ibmcom/secure-gateway-client
```
{: pre}

## Mac OS X
{: #mac}

Para eliminar el cliente de {{site.data.keyword.SecureGateway}} de Mac OS X, ejecute los mandatos siguientes desde el terminal:

```
cd /Applications
rm -rf ./ibm
```
{: codeblock}

## Linux
{: #linux}

### Ubuntu/PowerPC
{: #debian-uninstall}

Para buscar el estado de instalación del cliente de {{site.data.keyword.SecureGateway}},
puede utilizar el mandato siguiente:

```
sudo dpkg -l | grep ibm-securegateway-client
```
{: pre}

Para desinstalar el cliente de {{site.data.keyword.SecureGateway}}
sin eliminar el directorio de instalación, los archivos de configuración,
el id, el id de grupo y los archivos de registro, utilice el mandato siguiente:

```
sudo dpkg -r ibm-securegateway-client
```
{: pre}

Para desinstalar el cliente de {{site.data.keyword.SecureGateway}}
eliminando todo, utilice el mandato siguiente:

```
sudo dpkg --purge ibm-securegateway-client
```
{: pre}

### RedHat/SuSE
{: #rpm-uninstall}

Para buscar el estado de instalación del cliente de {{site.data.keyword.SecureGateway}},
puede utilizar el mandato siguiente:

```
rpm -qa | grep ibm-securegateway-client
```
{: pre}

Para desinstalar el cliente de {{site.data.keyword.SecureGateway}}
sin eliminar el directorio de instalación, los archivos de configuración,
el id, el id de grupo y los archivos de registro, utilice el mandato siguiente:

```
rpm -e ibm-securegateway-client
```
{: pre}

Para desinstalar el cliente de {{site.data.keyword.SecureGateway}}
eliminando todo, utilice el mandato siguiente:

```
yum remove ibm-securegateway-client
```
{: pre}

## Windows
{: #windows}

Hay dos maneras de eliminar la instalación de Windows para el cliente de {{site.data.keyword.SecureGateway}}:

1. Eliminar desde el directorio de instalación: vaya al directorio de instalación del cliente de {{site.data.keyword.SecureGateway}} y efectúe una doble pulsación en el archivo uninstall.exe.

2. Eliminar desde el panel de control: elimine la aplicación {{site.data.keyword.SecureGateway}} de la sección Programas del Panel de control.
