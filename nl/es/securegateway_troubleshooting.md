---

copyright:
  years: 2015, 2018
lastupdated: "2018-08-10"

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Resolución de problemas
{: #troubleshooting}

## Prácticas recomendadas para ejecutar el cliente de Secure Gateway
{: #best}

- Ejecute el cliente de Secure Gateway
en una partición del sistema operativo (OS) que tenga visibilidad de red
de los servicios enlazados con puente mediante el propio cliente. Por ejemplo,
algunos entornos de visualización alojados dan soporte a varias modalidades de conectividad de red,
incluidos NAT y Bridged. Asegúrese de elegir el tipo de conexión correcto
que le proporciona acceso a los servicios de {{site.data.keyword.Bluemix}}
desde Internet.
- Instale Secure Gateway en
el entorno de TI donde se lo permite la política de seguridad corporativa. Esto se encontraría
normalmente en una zona de color amarillo protegida o DMZ donde su empresa
pueda establecer los controles de seguridad adecuados para proteger los activos locales. Siga siempre las políticas e instrucciones de seguridad corporativas al instalar
el cliente de Secure Gateway.
- Antes de instalar un cliente en su entorno, asegúrese
de que tanto los activos locales como los de Internet estén accesibles
y que todos los nombres de host los resuelva un DNS.

## Pasos iniciales de la resolución de problemas

- Iniciar la solicitud fallida de la aplicación solicitante
- Consulte los registros del cliente de Secure Gateway
- Si no se ha generado ningún registro del cliente a partir de la solicitud, el problema se encuentra entre la aplicación solicitante y los servidores de Secure Gateway.  Esto puede abarcar desde un problema de fiabilidad de red, hasta protocolos de solicitud no coincidentes o un reconocimiento incorrecto de la autenticación mutua de TLS.
- Si el cliente ha generado registros de error a partir de la solicitud, el problema se encuentra entre el cliente de SG y el recurso local.  A continuación encontrará una tabla que contiene errores comunes, los problemas que suelen causarlos y los métodos potenciales para resolverlos.

Error | Causa típica | Métodos de resolución de problemas
--- | --- | ---
ETIMEDOUT | El cliente no puede encontrar el nombre de host/ip al que conectarse debido a restricciones de red. | Intente ejecutar ping del nombre de host/ip del destino desde el host que ejecuta el cliente para asegurar la conectividad de red.  Si ejecuta la versión Docker del cliente, es posible que se solucione el problema enlazando el contenedor con el sistema operativo del host con `--net=host`.
ECONNREFUSED | El cliente ha resuelto el nombre de host/ip al que se va a conectar, pero no puede iniciar el reconocimiento de la conexión | Esto suele estar causado por la falta de coincidencia de protocolo entre el cliente de SG y el recurso local (por ejemplo, el cliente está intentando una conexión TCP con un host:puerto que espera una conexión TLS).  En algunos casos, una regla de cortafuegos puede provocar este error en lugar de ETIMEDOUT.
ECONNRESET | El cliente ha establecido una conexión con el destino, pero algo ha salido mal durante el reconocimiento (un error de reconocimiento de TLS también puede dar lugar a otros errores) o mientras la solicitud estaba siendo gestionada por el recurso local. | Debe comprobar los registros del recurso local para confirmar que no haya ningún error que haya causado la interrupción de la conexión.  Si no se encuentra nada en los registros locales, se debe examinar la configuración de destino para asegurar que se proporcionan los protocolos adecuados (y los certificados, si es necesario) al cliente para la conexión.
REMOTE_RST | Se ha producido un error en el lado del servidor de SG. <br><br> Para un destino local, se ha producido un error cuando la app solicitante se ha conectado al servidor de SG o se ha producido un error de tiempo de espera excedido al recibir datos del recurso local. <br><br> Para un destino en la nube, podría ser cualquier cosa, desde un error de reconocimiento de TLS hasta errores en el recurso de nube | Para un destino local, asegúrese de que la app solicitante utiliza los protocolos adecuados para establecer la conexión con el servidor de SG. Si el error se produce al recibir datos del recurso local, intente ampliar/inhabilitar el tiempo de espera. <br><br> Para un destino en la nube, debe comprobar los registros del recurso en la nube para confirmar que no haya ningún error que haya causado la interrupción de la conexión.  Si no se encuentra nada en los registros del recurso en la nube, se debe examinar la configuración de destino para asegurar que se proporcionan los protocolos adecuados (y los certificados, si es necesario) al cliente para la conexión.

Muchas aplicaciones experimentan "bloqueos" después de que se produzca un ECONNRESET en el otro extremo del túnel. Este es el resultado esperado. Secure Gateway no puede reproducir el paquete RST en el otro extremo del túnel, ya que los paquetes TCP ya se han reconocido (ACKed) para ese lado del túnel. Los tiempos de espera excedidos de nivel de aplicación, la aplicación nunca recibe una respuesta de reconocimiento, es el único método para finalizar el bloqueo.

## Configuración del cliente Docker para que se reinicie cuando se reinicia el servidor
{: #docker}

### Qué está pasando
Al reiniciar el servidor donde se ejecuta el cliente de Secure Gateway,
debe reiniciar manualmente el cliente Docker de Secure Gateway. ¿Cómo puede hacer que el cliente se inicie automáticamente tras un
reinicio del sistema?

### Cómo solucionarlo

- En sistemas Linux o UNIX:
- Integre el mandato Docker en un script al que pueda llamar como resultado de un
trabajo CRON.
- Si utiliza una estación de trabajo Linux, puede crear una configuración inicial para garantizar que el cliente se inicie cada vez que se reinicia el servidor. Para obtener más información, consulte el apartado sobre inicio automático de contenedores en el sitio web
de Docker.
- En sistemas Windows,
emita el siguiente mandato para iniciar el cliente:

```
for /L %i in (0,0,0) do docker run -it ibmcom/secure-gateway-client <id_pasarela>
```
{: pre}

## Mensaje de error de conexión: el host no está en el CN del certificado
{: #not-in-cn}

### Qué está pasando
Está intentando implementar el TLS del lado del cliente local utilizando el cliente de Secure Gateway
y recibe el siguiente mensaje de error.

```
[ERROR] Connection #<connection ID> had error: Host: <host name>. is not cert's CN: <mycommonname>

Donde:
    - connection ID es el número de conexión asignado al cliente.
    - host name es el nombre de host para la conexión.
    - mycommonname es el FDQN o nombre común que se utiliza en el certificado.
```
{: screen}

### Por qué pasa esto
El Nombre común, por ejemplo, el FQDN del servidor o SU nombre,
entre la aplicación local y el certificado que ha subido
a {{site.data.keyword.Bluemix_notm}} para
este destino no coinciden.

- Compruebe los elementos siguientes:
- Que ha generado el certificado correctamente y que ha utilizado el
FQDN del servidor correcto o el nombre de host.
- Ha subido el certificado correcto al destino de {{site.data.keyword.Bluemix_notm}} para este cliente.

### Cómo solucionarlo

 1. En la interfaz de usuario de {{site.data.keyword.Bluemix_notm}}, vaya al panel de control de Secure Gateway.
 2. Seleccione el destino y pulse el icono Editar.
 3. Pulse Cargar certificado.
 4. Cargue el archivo de certificado de PEM que se utilizará para conectarse
al sistema local.

## La IP no está en la lista de certificados
{: #san}

### Qué está pasando
El CN del certificado presentado es la dirección IP de la pasarela, pero el certificado no tiene una SAN que coincida con la dirección IP y el cliente no se puede conectar.  

Debido a los problemas de resolución de nombre de host, estamos utilizando la dirección IP en nuestro destino.  El CN del certificado presentado es la dirección IP de la pasarela, pero el certificado no tiene una SAN que coincida con la dirección IP y el cliente no se puede conectar

Ha creado un destino utilizando TLS, pero en lugar de utilizar el nombre de host del destino, ha utilizado la dirección IP del mismo.  Al conectar el cliente, se genera el siguiente error.

```
[2015-10-15 13:00:04.866] [INFO] Connection #0 is being established to 10.3.20.31:443
[2015-10-15 13:00:05.426] [ERROR] Connection #0 to destination 10.3.20.31:443 had error: IP: 10.3.20.31 is not in the cert's list:
[2015-10-15 13:00:05.427] [INFO] Connection #0 to 10.3.20.31:443 was closed
```
{: screen}

### Por qué pasa esto
Lo que sucede es que el código de verificación SSL en el cliente de pasarela está tratando a este destino de forma distinta
porque utiliza una dirección IP en lugar de un nombre de host.  En lugar de coincidir con el CN del certificado,
está buscando en el SAN del certificado una coincidencia de la dirección IP.  Como no hay
ningún SAN en el certificado, lo ve como una conexión anómala y el reconocimiento SSL fallará.

### Cómo solucionarlo
Si se mira el mensaje de error, no indica el CN, ([ERROR] Connection ## had error: Host: . is not cert&apos;s CN: ), sino la lista del certificado, lo que me lleva a pensar que ha generado el certificado autofirmado incorrectamente. El problema está generando el certificado utilizando un FQDN o CN con una IP_Address. Esto no funcionará, ya que las direcciones IP solo reciben soporte cuando se utiliza SAN.

Método para generar un certificado con una dirección IP como el CN con openssl:

1. Cree un archivo de configuración de openssl; he copiado el mío de /usr/lib/ssl/openssl.cnf
2. Añada una sección alternate_names al archivo como en el ejemplo siguiente:

   ```
   [ alternate_names ]

   IP.1 = &lt;my application&apos;s ip&gt;
   ```
   {: codeblock}

3. En la sección [ v3_ca ], añada esta línea:

    ```
    subjectAltName = @alternate_names
    ```
    {: pre}

4. Bajo la sección CA_default, elimine el comentario de copy_extensions (opción de copia de extensión: utilícela con cuidado):

    ```
    copy_extensions = copy
    ```
    {: pre}

5. Genere la clave privada

    ```
    openssl genrsa -out private.key 3072
    ```
    {: pre}

6. Genere el certificado con opciones sobre la organización

    ```
    openssl req -new -x509 -key private.key -sha256 -out certificate.pem -days 730 -config
    ```
    {: pre}

7. Combine los archivos

    ```
    cat private.key certificate.pem > SAN.pem
    ```
    {: pre}

8. Cargue el archivo SAN.pem en el destino como certificado TLS del cliente.
9. Cargue el archivo SAN.pem en la aplicación local y reinicie.

10. Su destino se puede configurar para TCP, HTTP o HTTPS, y la aplicación del lado de la nube debe poder conectar con la aplicación local.

Si se encuentra con un problema UNABLE_TO_VERIFY_LEAF_SIGNATURE, compruebe el archivo openssl.conf y cambie lo
siguiente:

    ```
    keyUsage = digitalSignature, keyEncipherment
    ```
    {: pre}

al valor
predeterminado:

    ```
    keyUsage = nonRepudiation, digitalSignature, keyEncipherment
    ```
    {: pre}

## Mensaje de error de conexión: DEPTH_ZERO_SELF_SIGNED_CERT
{: #depth-zero}

### Qué está pasando
Está intentando implementar el TLS del lado del cliente local utilizando el cliente de Secure Gateway
y recibe el siguiente mensaje de error.

```
[ERROR] Connection #<connection ID> had error: DEPTH_ZERO_SELF_SIGNED_CERT Where:

    connection ID es un número de conexión asignado por el cliente.
```
{: screen}

### Por qué pasa esto
Al destino que ha definido le falta un certificado del lado del cliente.

### Cómo solucionarlo
 1. En la interfaz de usuario de {{site.data.keyword.Bluemix_notm}}, vaya al panel de control de Secure Gateway.
 2. Seleccione el destino y pulse el icono Editar.
 3. Pulse Cargar certificado.
 4. Cargue el archivo de certificado de PEM que se utilizará para conectarse
al sistema local.


## ¿Cómo puedo cargar de forma interactiva un archivo de ACL en el cliente de Docker?
{: #docker-acl}

### Qué está pasando
Puesto que Docker es un contenedor o un entorno virtualizado, no tiene acceso directo
al sistema de archivos hasta que se inicia realmente el contenedor.  Esto le impide leer el sistema de archivos de las máquinas del host
hasta que se haya iniciado y esté realmente en ejecución.

### Cómo solucionarlo
Esto es lo que puede hacer:

- Cree un archivo Docker que incluya el archivo aclfile.txt

```
FROM ibmcom/secure-gateway-client
ADD aclfile.txt /tmp/aclfile.txt
```
{: codeblock}

- Cree una nueva imagen de docker

```
docker build -t ads-secure-gateway-client .
```
{: pre}

- Ejecute la nueva imagen de docker (tiene que especificar las opciones -t e -i; de lo contrario, recibirá un error que indica que no se encuentra el archivo o ENOENT):

```
docker run -t -i ads-secure-gateway-client1 --F /tmp/aclfile.txt
```
{: pre}

- Se obtiene la salida siguiente:

```
[2015-09-30 16:50:32.084] [INFO] The current access control list is being reset and replaced by the user provided file: /tmp/aclfile.txt
[2015-09-30 16:50:32.086] [INFO] The ACL file process accepts acl allow :8000
[2015-09-30 16:50:32.087] [INFO] The ACL file process accepts acl deny local
```
{: screen}

## Obtención de ayuda y soporte adicionales
{: #support}

Si tiene preguntas técnicas sobre el desarrollo o el despliegue de una aplicación con Secure Gateway, publique su pregunta en [Stack Overflow ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo") ](http://stackoverflow.com/search?q=securegateway+ibm-bluemix).  Etiquete la pregunta con "ibm-bluemix" y "secure-gateway" para que los equipos de desarrollo de {{site.data.keyword.Bluemix_notm}} puedan encontrarla con facilidad.

Si tiene preguntas sobre el servicio o las instrucciones sobre cómo empezar, utilice el foro [IBM developerWorks dW Answers ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo") ](https://developer.ibm.com/answers/topics/securegateway/?smartspace=bluemix) junto con las etiquetas "bluemix" y "Secure Gateway".

Para obtener más información sobre cómo utilizar estos foros, consulte la [página de obtención de ayuda aquí](https://console.ng.bluemix.net/docs/support/index.html#getting-help).

Para obtener información sobre cómo abrir una incidencia de soporte de IBM, o sobre los niveles de soporte y la gravedad de las incidencias, consulte [Cómo obtener soporte](https://console.ng.bluemix.net/docs/support/index.html#contacting-support).

Proporcione toda la información siguiente posible cuando envíe una incidencia:

- ¿En qué SO se ejecuta el cliente?
- ¿Qué versión del cliente se utiliza (lo encontrará con el mandato 'C' en el cliente)?
- Si se trata de un problema de IU, pegue o adjunte los registros y capturas de pantalla asociados de la consola del navegador
- Pegue o adjunte cualquier registro de aplicación solicitante asociado
- Pegue o adjunte cualquier registro de cliente de Secure Gateway asociado
- Proporcione los detalles del destino que se está utilizando (en una captura de pantalla o rellenando los campos siguientes):
   - ID de destino
   - Protocolo
   - Autenticación del lado del destino
   - Certificados cargados (solo los nombres y el recuadro en el que se han cargado)
