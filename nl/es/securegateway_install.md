---

copyright:
  years: 2015, 2019
lastupdated: "2019-05-07"

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Instalación del cliente
{: #client-install}

## Docker
{: #installing-docker}

Docker
es una plataforma de otro proveedor que ofrece un contenedor para instalar aplicaciones de forma rápida y sencilla con poca o ninguna configuración. El servicio {{site.data.keyword.SecureGateway}} proporciona una imagen
del Docker que se puede utilizar una vez instalado el programa de utilidad Docker en la estación de trabajo.  Para instalar Docker, consulte el sitio web de instalación de Docker y siga las instrucciones correspondientes a su sistema.

### Instalación y actualización del cliente
{: #docker-install}

La instalación y la actualización de la imagen de Docker del cliente de {{site.data.keyword.SecureGateway}} utilizan el mismo mandato:

```
docker pull ibmcom/secure-gateway-client
```
{: pre}

### Inicio de una sesión de cliente interactiva
{: #docker-run}

Para ejecutar el contenedor de Docker para IBM {{site.data.keyword.SecureGateway}}, emita el mandato siguiente:

```
docker run -it ibmcom/secure-gateway-client <gateway ID> -t <security token>
```
{: pre}

Normalmente se necesitan dos parámetros: el ID de pasarela de {{site.data.keyword.SecureGateway}} y la señal de seguridad de la pasarela, que están disponibles en el panel de control de {{site.data.keyword.SecureGateway}}.

### Mandatos Docker soportados
{: #docker-commands}

El cliente de {{site.data.keyword.SecureGateway}} solo da soporte a los mandatos `pull` y `run` para manipular el contenedor.

Volver a [Iniciación - Adición de un cliente](/docs/services/SecureGateway?topic=securegateway-add-client).

## Mac OS X
{: #installing-mac}

### Requisitos para la ejecución en Mac OS X
{: #mac-requirements}

Es necesario cumplir los siguientes requisitos previos antes de ejecutar el cliente en Mac OS X.
 1. Instale las herramientas de desarrollo de Xcode, que necesitan algunos de los módulos de nodo de esta plataforma.

### Procedimiento de instalación en Mac OS X
{: #mac-install}

Es posible que necesite privilegios administrativos para realizar esta instalación, en función de la configuración de seguridad del sistema.

 1. Monte la imagen de DMG que se ha descargado desde la IU de {{site.data.keyword.SecureGateway}}; generalmente es necesario realizar una doble pulsación en la misma.
 2. Debería aparecer una nueva ventana del 'buscador'. Si no es así, realice una doble pulsación en el volumen montado. Esta ventana debería contener un icono de carpeta "ibm" y un icono de "acceso directo" a las aplicaciones; arrastre y suelte la carpeta "ibm" en el acceso directo.

### Inicio de una sesión de cliente interactiva
{: #mac-run}

Para iniciar el cliente, ejecute el archivo `secgw.command` que se encuentra en la ubicación de instalación predeterminada: `/Applications/ibm/`.

Volver a [Iniciación - Adición de un cliente](/docs/services/SecureGateway?topic=securegateway-add-client).

## Linux
{: #installing-linux}

La instalación incluye el cliente de {{site.data.keyword.SecureGatewayfull}}, así como una versión segura del paquete de nodejs de IBM.  Ambos se instalan bajo el directorio /opt/ibm en el sistema.  El instalador creará o actualizará lo siguiente:

| Directorio | Nombre de archivo | Descripción          |
| ------------- | ------------- | ----------- |
| /etc/ibm | sgenvironment.conf | archivo de configuración para upstart |
| /etc/ibm | passwd | crea el usuario: secgwadmin:x:501:501::/home/secgwadmin:/bin/bash |
| /etc/ibm | group | crea el grupo: secgwadmin:x:501: |
| /etc/init | securegateway_client.conf | archivo upstart |
| /opt/ibm | node-v&lt;version&gt;-linux-x64 | Instalación de Node JS de IBM |
| /opt/ibm | securegateway | instalación del cliente de {{site.data.keyword.SecureGateway}} |
| /usr/local/bin | node | symlink creado para el ejecutable de nodo de IBM |
| /usr/local/bin | npm | symlink creado para el ejecutable npm de IBM |
| /var/log/securegateway | client_console.log | archivo de registro creado cuando se ejecuta el cliente como un proceso de inicio automático |

Necesita privilegios de root o administrativos para realizar esta instalación.

### Instalación en Ubuntu/PowerPC
{: #debian-install}

 1. Instale el cliente de {{site.data.keyword.SecureGateway}}. Por ejemplo, si utiliza un paquete de Debian, emita el mandato siguiente.

    ```
    sudo dpkg -i <secure-gateway-debian-package>
    ```
    {: pre}

 2. Cuando se inicie el instalador del cliente, se le solicitará la
siguiente información.

    <b>Proceso de inicio automático Sí/No</b>
Si esta es una actualización o una reinstalación, debe elegir si desea que el cliente existente se detenga
mientras esté en ejecución el proceso de instalación. Elija S/s para detener el
cliente existente. De lo contrario, se actualizará el paquete del cliente sin reiniciar el cliente, lo que significa
que puede esperar a que una ventana de actualización de software adecuada realice el reinicio. Si selecciona
N/n, la instalación continuará y deberá reiniciar el cliente
manualmente.

    <b>ID de pasarela</b>
Establezca el ID de pasarela para el cliente. El ID de pasarela estará definido al crear un servicio de {{site.data.keyword.SecureGateway}}. Si el cliente no se puede conectar,
podrá cambiar la selección editando el archivo .conf.

    <b>Señal de seguridad</b>
Si el ID de pasarela está habilitado para comprobar si hay una señal de seguridad, debe especificarla ahora. Si el
ID de pasarela no requiere una señal de seguridad, déjelo en blanco.

    <b>Nivel de registro</b>
El valor predeterminado es INFO. Otros valores válidos son TRACE, DEBUG y ERROR.

    <b>Lista de control de accesos</b>
Especifique la vía de acceso absoluta del nombre de archivo que contiene la lista de control de accesos sobre lo que tiene acceso a los recursos locales. Para obtener más información, consulte el archivo README.md en /opt/ibm/securegateway o la lista de control de accesos.

    <b>IU de cliente</b>
Elija si desea utilizar la interfaz de usuario del cliente. Si es así, el usuario puede cambiar el puerto de la interfaz de usuario. El valor predeterminado es 9003.

    <b>Nota:</b> no tiene que responder a ninguna de las solicitudes; todas adoptarán el valor predeterminado definido o se dejarán en blanco en el archivo sgenvironment.conf. Esto permite que el proceso de instalación se ejecute
sin interacción del usuario.

    <b>Nota:</b> el archivo sgenvironment.conf se lee cada vez que se inicia o se reinicia el cliente utilizando el proceso de inicio del sistema. Puede editar el archivo /etc/ibm/sgenvironment.conf siempre que lo desee para realizar cambios en la configuración y reiniciar el cliente para que se adopte dichos cambios.

    <b>Nota:</b> el idioma de los registros de servicio de cliente de {{site.data.keyword.SecureGateway}} se puede cambiar modificando el parámetro `LANGUAGE` en el archivo /etc/ibm/sgenvironment.conf. Los registros de servicio cambiarán al idioma seleccionado después de reiniciar el servicio.

 3. Si reinicia el cliente, para asegurarse de que el cliente se esté ejecutando correctamente
emita el mandato siguiente:

    ```
    cat /var/log/securegateway/client_console.log
    ```
    {: pre}

 4. Si desea editar el archivo de configuración, para asegurarse de que el archivo de configuración se actualiza correctamente emita el mandato siguiente:

    ```
    sudo vi /etc/ibm/sgenvironment.conf
    ```
    {: pre}

 5. Para comprobar el estado de la instalación del cliente, emita
el siguiente mandato:

    ```
    sudo dpkg --status ibm-securegateway-client
    ```
    {: pre}

Un ejemplo del resultado que se devuelve:

```
Package: ibm-securegateway-client
Status: install ok installed
Priority: extra
Section: IBM/net
Maintainer: cloudoe-dev@webconf.ibm.com
Architecture: amd64
Source: ibm-securegateway+securegateway-client
Version: 1.7.0
Description: IBM Secure Gateway Client for Bluemix
IBM Secure Gateway client package for Bluemix, supports secure connections to on-premises connections.
Homepage: https://ng.bluemix.net/
License: http://www.ibm.com/software/sla/sladb.nsf/lilookup/986C7686F22D4D3585257E13004EA6CB?OpenDocument
```
{: screen}

### Instalación en RedHat/SuSE
{: #rpm-install}

1. Instale el cliente de {{site.data.keyword.SecureGateway}}. Por ejemplo, si utiliza un paquete de RPM, emita el mandato siguiente:

   ```
   rpm -ivhf <secure-gateway-rpm-package>
   ```
   {: pre}


   El instalador del cliente inicia e instala el cliente, y crea un archivo sgenvironment.conf en /etc/ibm.

2. Opcional: si desea utilizar el proceso upstart del sistema, debe editar este archivo y especificar lo siguiente para que el cliente se inicie correctamente. Consulte [Utilización de upstart](/docs/services/SecureGateway?topic=securegateway-auto-start-conf#auto-start-linux) para obtener más información sobre cómo editar este archivo de configuración.

3. Si ha iniciado el cliente mediante upstart, compruebe el archivo de registro para asegurarse de que se está ejecutando correctamente.

   ```
   cat /var/log/securegateway/client_console.log
   ```
   {: pre}

4. Para comprobar el estado de la instalación del cliente, emita
el siguiente mandato:

   ```
   rpm -q ibm-securegateway-client
   ```
   {: pre}

### Instalación de AIX
{: #aix-install}

1. Asegúrese de que el permiso ejecutable está establecido para el paquete. Si es necesario, cambie las autorizaciones del archivo con el mandato siguiente:
    ```
    chmod a+x <secure-gateway-bin-package>
    ```
2. Extraiga el paquete con el siguiente mandato:
    ```
    ./<secure-gateway-bin-package>
    ```

Nota: asegúrese de que el sistema AIX satisface el requisito para
ejecutar Node.js y de que ksh esté instalado.

### Inicio de una sesión de cliente interactiva
{: #linux-run}

Para iniciar el cliente, ejecute los mandatos siguientes:

```
cd /opt/ibm/securegateway/client
node lib/secgwclient.js <gateway ID> -t <security token>
```
{: codeblock}

Normalmente se necesitan dos parámetros: un ID de pasarela de {{site.data.keyword.SecureGateway}} y la señal de seguridad de la pasarela, que están disponibles en el panel de control de {{site.data.keyword.SecureGateway}}.

Volver a [Iniciación - Adición de un cliente](/docs/services/SecureGateway?topic=securegateway-add-client).

## Windows
{: #installing-windows}

### Instalación del cliente
{: #windows-install}

Es posible que necesite privilegios administrativos para realizar esta instalación, en función de la configuración de seguridad del sistema.

 1. Copie el archivo del instalador EXE en el sistema.  También se proporcionará MD5 sum, que puede utilizar para comprobar
la integridad del paquete de instalación.

 2. Para instalarlo, efectúe una doble pulsación en el mismo y responda a las solicitudes, o ejecute el archivo ejecutable desde el indicador de mandatos.

 3. Se solicitará al usuario que elija el directorio de instalación que desee. La ubicación predeterminada es el directorio Archivos de programa (x86).

 4. Se solicitará al usuario que seleccione el idioma en el que desea iniciar la interfaz de línea de mandatos. Si no se selecciona el idioma, se adoptará el inglés como valor predeterminado.

 5. Se solicitará al usuario que instale el cliente como un servicio de Windows. Si el usuario elige hacerlo, el cliente se ejecutará en segundo plano como un servicio de Windows con las configuraciones que especifique el usuario en el diálogo siguiente. El estado del servicio se puede comprobar en la página de servicio del Panel de control. El nombre del servicio es "IBM Bluemix Secure Gateway Service".

 6. El instalador identifica si hay una instalación existente en la máquina. Si se detecta, se pregunta al usuario si desea utilizar las configuraciones existentes; si no lo desea, el usuario tiene la opción de especificar nuevas configuraciones. Aquí se pueden especificar detalles como los ID de pasarela, las señales de seguridad, los archivos acl y el nivel de registro para cada pasarela. Si el usuario ha elegido ejecutar el cliente como un servicio de Windows, el cliente se iniciará con las configuraciones proporcionadas. Si el usuario no ha elegido iniciar el cliente como un servicio de Windows, las configuraciones se guardarán para su uso posterior. En cualquiera de los casos, las configuraciones se guardan en %Installation_directory%/ibm/securegateway/client/securegw_service.config.

 7. A partir de la versión 1.5.0, el cliente se suministra con una interfaz de usuario de cliente completamente funcional. Puede configurarla desde el propio instalador. El usuario puede especificar la contraseña y el número de puerto (el valor predeterminado es 9003) a partir del cual se iniciará la interfaz de usuario del cliente.

 Si el usuario elige iniciar el cliente con conexión con varias pasarelas, tenga cuidado al especificar los valores de configuración. Los ID de pasarela tienen que estar separados por espacios. Las señales de seguridad, los archivos acl y los niveles de registro deben estar delimitados por --. Si no desea especificar ninguno de estos tres valores para una pasarela determinada, utilice 'none'.

 <b>Nota:</b> asegúrese de que no haya espacios en blanco residuales.

 <b>Nota:</b> tenga en cuenta que los archivos acl se deben colocar en el directorio %Installation_directory%/ibm/securegateway/client o en uno relativo a esta vía de acceso.

### Inicio de una sesión de cliente interactiva
{: #windows-run}

Para iniciar el cliente, abra un indicador de mandatos con privilegios de administración y ejecute los mandatos siguientes:

```
cd "<Installation_directory>\ibm\securegateway\client"
secgw.cmd
```
{: codeblock}

De forma alternativa, vaya a `<Installation_directory>\ibm\securegateway\client` y efectúe una doble pulsación en `secgw.cmd`.

<b>Nota:</b> puede elegir utilizar las configuraciones que están almacenadas en el archivo `<Installation_directory>\ibm\securegateway\client\securegw_service.config` o proporcionar los detalles de forma interactiva.

Volver a [Iniciación - Adición de un cliente](/docs/services/SecureGateway?topic=securegateway-add-client).

## DataPower
{: #installing-datapower}

DataPower tiene una versión incorporada del cliente de {{site.data.keyword.SecureGateway}}.  En función de la versión de DataPower, es posible que tenga una versión distinta del cliente de {{site.data.keyword.SecureGateway}}.  Tenga en cuenta las [limitaciones del cliente de DataPower](/docs/services/SecureGateway?topic=securegateway-client-interacting#limits-datapower) aplicables. Si se utiliza del cliente de Secure Gateway antiguo pueden producirse errores inesperados.

| Versión de DataPower | Versión del cliente de {{site.data.keyword.SecureGateway}}  |
| -- | --  |
| 7.2.0.0, 7.5.0.0 | 1.1.0  |
| 7.5.1.0, 7.7.0 | 1.4.2  |
| 7.5.2.4 | 1.6.1  |
| 7.5.2.6, 7.6.0.0 | 1.7.0  |
| 7.5.2.14, 7.6.0.7, 7.7.1.0, 2018.4.1.0 |  1.8.0fp6  |
| 2018.4.1.4 | 1.8.2  |
| 7.6.0.15, 2018.4.1.6 | 1.8.2fp1 |

### Inicio de una sesión de cliente
{: #datapower-run}

1. Inicie una sesión en la GUI web de DataPower
2. Vaya a Objetos > Nube > Cliente de Secure Gateway o busque el cliente de Secure Gateway
3. Pulse `Añadir` para configurar una nueva conexión de cliente
4. Especifique un nombre, el ID de pasarela y la señal de seguridad (si es aplicable) y luego aplique los cambios.

Volver a [Iniciación - Adición de un cliente](/docs/services/SecureGateway?topic=securegateway-add-client).
