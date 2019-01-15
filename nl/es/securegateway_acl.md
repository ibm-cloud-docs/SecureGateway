---

copyright:
  years: 2015, 2017
lastupdated: "2017-04-10"

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Lista de control de accesos

El cliente de {{site.data.keyword.SecureGateway}} proporciona soporte integrado de ACL (lista de control de accesos). Puede permitir o restringir (denegar) el acceso a los recursos locales realizando modificaciones en la ACL para el cliente.  Esto se puede llevar a cabo de forma interactiva utilizando los mandatos del cliente o especificando un archivo que contenga las ACL que desea tener en vigor.

A partir de la versión 1.5.0, las reglas de lista de control de accesos se sincronizarán entre todos los clientes conectados a la misma pasarela.  De este modo, solo tiene que establecer/actualizar su ACL desde un solo cliente y se compartirá entre todos los clientes en ejecución conectados a dicha pasarela.  La ACL también persistirá en todas las sesiones, como por ejemplo conectando un nuevo cliente también aplicará las mismas reglas de ACL.

## Mandatos de lista de control de accesos
{: #commands}

Los mandatos de ACL soportados son:

```
acl allow [<hostname>]:[<port>]
acl deny [<hostname>]:[<port>]
no acl [<hostname>]:[<port>]
no acl
show acl
```
{: screen}

En los formularios en los que no se especifica un nombre de host o un puerto, se utilizan todos los nombres de host o puertos.  Por ejemplo, esta regla de ACL permite todos los nombres de host para el puerto 22.

```
acl allow :22
```
{: pre}

Este es otra regla de ACL de ejemplo que permite todos los nombres de host para todos los puertos, básicamente inhabilitando el soporte de ACL. <b>No se recomienda esta opción. </b>

```
acl allow :
```
{: pre}

El mandato `show acl` mostrará la ACL establecida actualmente o proporcionará un mensaje sobre el valor global.

Volver a [Iniciación - Adición de un cliente](./securegateway_client.html).

## Control de rutas HTTP/S mediante la ACL
{: #routes}

A partir de la versión 1.6.0, los destinos HTTP/S también pueden imponer rutas específicas en las entradas de ACL.  Estas se añaden del mismo modo que las entradas de ACL típicas, pero se añade la vía de acceso al final de la regla. Por ejemplo, esto solo permitirá el paso de las solicitudes que sigan la vía de acceso /my/api:

```
acl allow localhost:80/my/api
```
{: pre}

Con esta regla en vigor, se permitirá el paso a las solicitudes destinadas a `<cloud host>:<cloud port>/my/api*`.

Las rutas solo reciben soporte en los mandatos `acl allow`.

## Precedencia de lista de control de accesos
{: #precedence}

Después de especificar el mandato `acl allow :`, si se especifican otros mandatos `acl allow`, la regla para permitir `ALL:ALL` (procedente de `acl allow :`) se eliminará de la lista ya que se presupondrá que ya no desea permitir el acceso ilimitado.  Después de especificar el mandato `acl deny :`, si se especifica otro mandato `acl deny`, la regla para denegar `ALL:ALL` (procedente de `acl deny :`) se eliminará de la lista ya que se presupondrá que ya no desea restringir todo el acceso.  Si lista las reglas de ACL actuales mediante el mandato `show acl` en la CLI, habrá un indicador para visualizar si las reglas no listadas se permiten o deniegan.

## Importación de un archivo ACL
{: #import}

Puede proporcionar un nombre de archivo al mandato `acl file` que contenga los mandatos de ACL soportados que el cliente leerá cuando se inicie. Este archivo debe contener mandatos con el formato siguiente:

```
acl allow [<hostname>]:[<port>]
acl deny [<hostname>]:[<port>]
no acl [<hostname>]:[<port>]
no acl
```
{: screen}

<b>Nota:</b> `noacl ` sin ningún otro parámetro RESTABLECE la tabla de ACL y establece el acceso en DENY ALL.

[Aquí](./securegateway_acl-file.html) encontrará un archivo ACL de ejemplo.

Volver a [Iniciación - Adición de un cliente](./securegateway_client.html).

## Copia del archivo ACL en el cliente de Docker de {{site.data.keyword.SecureGateway}}
{: #docker}

El cliente docker de {{site.data.keyword.SecureGateway}} se ejecuta básicamente en su propio contenedor de virtualización.  Por lo tanto, el sistema de archivos de la máquina que lo aloja no es directamente accesible para los procesos que se ejecutan dentro del contenedor, incluido el cliente de {{site.data.keyword.SecureGateway}}.  A partir de la versión 1.8.0 del Motor de Docker, puede utilizar el mandato
'docker cp' para enviar por push archivos existentes en el host en el contenedor
mientras se está ejecutando o se está detenido.  Esto se debe llevar a cabo para poder utilizar el mandato interactivo FILE de ACL del cliente de {{site.data.keyword.SecureGateway}}.

Para utilizar el soporte interactivo de 'cp' en el docker desde el host a la instancia de docker, debe tener docker 1.8.0. Para comprobarlo, utilice `docker --version`

Una vez que haya hecho esto, su versión debería mostrarse como se indica a continuación. Se recomienda que permita que docker se ejecute como usuario no root, por lo que ejecute el mandato que se recomienda después de haber actualizado el motor a la versión 1.8.0 o 1.8.2.

```
Client:
 Version:      1.8.2
 API version:  1.20
 Go version:   go1.4.2
 Git commit:   0a8c2e3
 Built:        Thu Sep 10 19:21:21 UTC 2015
 OS/Arch:      linux/amd64

Server:
 Version:      1.8.2
 API version:  1.20
 Go version:   go1.4.2
 Git commit:   0a8c2e3
 Built:        Thu Sep 10 19:21:21 UTC 2015
 OS/Arch:      linux/amd64
```
{: screen}

Luego, para extraer la lista de archivos acl a la imagen del docker, siga los pasos siguientes:

- Ejecute el mandato 'docker ps' para buscar el ID del contenedor.

```
CONTAINER ID IMAGE                        COMMAND                CREATED        STATUS  PORTS NAMES
764aadce386b ibmcom/secure-gateway-client "node lib/secgwclient" 27 seconds ago Up 26 seconds condescending_nobel
```
{: screen}

- Copie el archivo acl.list con el mandato 'docker cp' utilizando el nombre o el ID de contenedor:

```
docker cp 01_client.list 764aadce386b:/root/01_client.list
```
{: pre}

- A continuación, en el cliente de {{site.data.keyword.SecureGateway}} que se ejecuta en docker:

```
cli F /root/01_client.list

 [2015-10-01 08:12:30.091] [INFO] The current access control list is being reset and replaced by the user provided file: /root/01_client.list
 [2015-10-01 08:12:30.093] [INFO] The ACL file process accepts acl allow 127.0.0.1:27017
 [2015-10-01 08:12:30.094] [INFO] The ACL file process accepts acl allow 127.0.0.1:22
```
{: screen}

- Visualice la ACL:

```
cli> S
 -------------------------------------------------------------------
           -- Secure Gateway Client Access Control List --          

  hostname                               port                  value
  127.0.0.1                             27017                  Allow
  127.0.0.1                                22                  Allow
 -------------------------------------------------------------------
```
{: screen}

Volver a [Iniciación - Adición de un cliente](./securegateway_client.html).
