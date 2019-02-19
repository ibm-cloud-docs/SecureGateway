---

copyright:
  years: 2015, 2017
lastupdated: "2017-04-25"

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Configuración de inicio automático para el cliente
{: #auto-start-conf}

## Linux
{: #linux}

Si ha optado por utilizar el recurso de inicio automático del sistema, utilice uno de los métodos siguientes para iniciar el cliente.  El archivo de configuración que utilizará el cliente se puede encontrar en:

```
/etc/ibm/sgenvironment.conf
```
{: screen}

<b>Nota:</b> la función de inicio automático no está disponible para Red Hat Linux 6.

Este archivo incluye las siguientes variables importantes que se deben establecer:

| Variable de entorno | Descripción       |
| ------------- | ----------- |
| RESTART_CLIENT | Iniciar y reiniciar el cliente durante la instalación o la actualización (Sí o No) |
| GATEWAY_ID | El ID de pasarela que se ha creado en la interfaz de usuario de Secure Gateway for Bluemix |
| SECTOKEN | Señal de seguridad para este ID de pasarela, si elige imponer la seguridad al crearla |
| ACL_FILE | Archivo de lista de control de accesos que desee utilizar para restringir el acceso local a los recursos |
| LOGLEVEL | El nivel de registro que desea establecer para el servicio (el valor predeterminado es INFO) |
| USE_UI   | Establezca este valor en 'N' si no desea iniciar la interfaz de usuario del cliente |
| UI_PORT  | El puerto en el que desea iniciar la interfaz de usuario del cliente (el valor predeterminado es 9003) |
| LANGUAGE | El idioma en el que desea tener los registros de cliente (el valor predeterminado es en) |

<b>Nota:</b> este archivo será de solo lectura si está utilizando el recurso de inicio automático del sistema.  Si está ejecutando
el cliente de forma manual, se omitirá este archivo.

### Upstart
{: #upstart}

### Iniciar el cliente
{: #upstart-start}

Si utiliza la función upstart para que el cliente de Secure Gateway se ejecute automáticamente al arrancar el sistema, primero tiene que comprobar su configuración (/etc/ibm/sgenvironment.conf).  Una vez que haya configurado el servicio upstart, puede utilizar el mandato siguiente para iniciarlo:

```
sudo initctl start securegateway_client
```
{: pre}

Para reiniciar el servicio:

```
sudo initctl restart securegateway_client
```
{: pre}

### Detención del cliente
{: #upstart-stop}

Para detener el servicio, ejecute el mandato siguiente:

```
sudo initctl stop securegateway_client
```
{: pre}

### SystemD
{: #systemd}


### Iniciar el cliente
{: #systemd-start}

Si utiliza la función systemD para que el cliente de Secure Gateway se ejecute automáticamente al arrancar el sistema, tiene que comprobar su configuración (/etc/ibm/sgenvironment.conf).  Una vez que haya configurado el servicio upstart, puede utilizar el mandato siguiente para iniciarlo:

```
systemctl start securegateway_client
```
{: pre}

Para reiniciar el servicio:

```
systemctl restart securegateway_client
```
{: pre}

Para ver el estado del servicio:

```
systemctl status securegateway_client
```
{: pre}

### Detención del cliente
{: #systemd-stop}

Para detener el servicio, ejecute el mandato siguiente:

```
systemctl stop securegateway_client
```
{: pre}

### SystemV
{: #systemv}

System V no se configura durante la instalación
como el resto de los recursos de inicio automático. Los scripts están disponibles en el
directorio de instalación /opt/ibm/securegateway/client/upstart y son los siguientes:

```
secgw.sh
securegateway_clientd
```
{: screen}

<b>Nota:</b> esta sección afecta a los usuarios de SuSE/SLES 11.

Opcional: si está ejecutando SuSE versión 11 que utiliza el inicio automático de systemV, se proporcionan scripts que se pueden utilizar para configurar este proceso. Siga este procedimiento:

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

Cuando se hayan ejecutado estos pasos, se pueden utilizar los mandatos YasT y systemV para iniciar/detener el daemon.

Volver a [Iniciación - Adición de un cliente](/docs/services/SecureGateway/securegateway_client.html).

## Windows
{: #windows}

Para modificar el estado del servicio Windows, abra una ventana de mandatos con privilegios de administrador.

```
cd "<Installation_directory>\ibm\securegateway\client"
```
{: pre}

Si el servicio ya se está ejecutando, el usuario puede utilizar el siguiente mandato para detenerlo

```
windowsService.cmd uninstall
```
{: pre}

Para iniciar el servicio, utilice el mandato siguiente

```
windowsService.cmd install
```
{: pre}

<b>Nota:</b> el servicio se ejecutará basándose en las configuraciones almacenadas en:

```
%Installation_directory%/ibm/securegateway/client/securegw_service.config
```
{: pre}

Los registros de la aplicación para el servicio Windows se almacenarán en:

```
%Installation_directory%/ibm/securegateway/client/logs/securegw_win_service.log
```
{:pre}

 Los registros se guardan en un nuevo archivo a diario.

Volver a [Iniciación - Adición de un cliente](/docs/services/SecureGateway/securegateway_client.html).
