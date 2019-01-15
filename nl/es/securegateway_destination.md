---

copyright:
  years: 2015, 2017
lastupdated: "2017-04-10"

---

# Adición de un destino

Un destino es una definición de cómo conectar con un determinado recurso local o en la nube. Una vez creado el destino, los servidores de {{site.data.keyword.SecureGateway}} le proporcionarán un punto final público exclusivo donde escuchará si hay conexiones mientras la pasarela esté conectada.

![Panel de control sin ningún destino](./images/emptyDestinations.png?raw=true "Panel de control sin ningún destino")

Desde dentro de la nueva pasarela y en el separador Destinos, pulse el botón Añadir destino para abrir el panel Añadir destino.  Hay dos métodos para crear su destino: una configuración guiada que muestra cómo se ajusta cada pieza en la arquitectura global y una configuración avanzada que proporciona todos los campos y opciones en un solo panel.  

La configuración guiada <b>no</b> permite la configuración de la información de proxy, los indicadores de nombre de servidor ni la carga de un par de certificado/clave específico de un destino.  Tras la creación, todos los campos están disponibles en el panel Editar destino.

## Panel guiado

![Configuración guiada](./images/guidedLanding.png?raw=true "Panel inicial de la configuración guiada")

## Panel avanzado

![Configuración avanzada](./images/advancedLanding.png?raw=true "Panel inicial de la configuración avanzada")

## Destinos locales frente a destinos en la nube
{: #dest-types}

La primera pregunta que se debe responder al crear el destino es dónde reside el recurso al que se debe conectar.

### Destino local
El destino local es para el caso en el que una aplicación del espacio público necesita acceder a un recurso restringido ubicado en el entorno local. ![Destino local](./images/onPremDestination.png?raw=true "Destino local")

### Destino en la nube
El destino en la nube es para el caso en el que una aplicación ubicada en una red restringida necesita acceder a un recurso que está disponible en un espacio público. ![Destino inverso](./images/reverseDestination.png?raw=true "Destino en la nube")

## Definición del destino
Para ambos tipos de destinos, es necesaria la información siguiente:

- <b>Nombre de host del recurso</b>: es la dirección IP o el nombre de host del recurso al que se debe conectar.
- <b>Puerto del recurso</b>: es el puerto en el que escucha el recurso.
- <b>Protocolo</b>: es el tipo de conexión que va a realizar la aplicación.  Consulte la tabla siguiente para ver las distintas [opciones de protocolo](#protocols).  Para configurar el tipo de conexión que espera el recurso, consulte la sección [Autenticación de recursos](#resource-auth).

Si ha seleccionado un destino en la nube, también tendrá que especificar un <b>Puerto de cliente</b>.  Es el puerto en el que escuchará el cliente de {{site.data.keyword.SecureGateway}} para permitir conexiones al nombre de host y puerto del recurso asociado.

## Opciones de protocolo
{: #protocols}

Esta tabla contiene todas las opciones disponibles para la forma en que la aplicación puede iniciar conexiones/solicitudes con {{site.data.keyword.SecureGateway}}.

Protocolo | Descripción
-- | --
TCP | Sin autenticación. La aplicación se puede conectar directamente con los servidores de {{site.data.keyword.SecureGateway}} sin necesidad de certificados.
TLS: Lado del servidor | La seguridad de la capa de transporte (TLS) está habilitada y el servidor proporciona un certificado para demostrar su autenticidad.
TLS: Autorización mutua | El servidor proporciona un conjunto de certificados y la aplicación también debe proporcionar un certificado como parte de su conexión.
HTTP | Conexión TCP donde se vuelve a escribir la cabecera de host para que coincida con el nombre de host local.
HTTPS | TLS: Conexión del lado del servidor donde se vuelve a escribir la cabecera de host para que coincida con el nombre de host local.
HTTPS: Autorización mutua | TLS: Conexión de autorización mutua donde se vuelve a escribir la cabecera de host para que coincida con el nombre de host local.


## Configuración de la autenticación mutua
{: #mutual-auth}

Para los protocolos que imponen la autenticación mutua, deberá cargar su propio certificado o el servidor creará automáticamente un par de certificado/clave autofirmado para que lo utilice la aplicación.  Este par se puede descargar junto con el certificado del servidor. ![Paneles de autenticación mutua](./images/mutualAuth.png?raw=true "Paneles de autenticación mutua")

### Autenticación de usuarios
{: #user-auth}

La sección de autenticación de usuario es para gestionar la autorización de la aplicación solicitante/de conexión con {{site.data.keyword.SecureGateway}}.  Este campo acepta un solo certificado, que debe ser el certificado que presentará su aplicación junto a cualquier conexión/solicitud.

### Autenticación de recursos
{: #resource-auth}

La autenticación de recursos determina cómo {{site.data.keyword.SecureGateway}} intentará conectar con el recurso definido.  Hay tres opciones disponibles: Ninguna, TLS (lado del servidor) y Autenticación mutua. En función de su elección, estarán disponibles distintas opciones de autenticación.

La habilitación de TLS en la conexión con el recurso es independiente del TLS utilizado para la autenticación de usuarios.  TLS para la autenticación de usuarios protege el acceso entre la aplicación solicitante inicial y {{site.data.keyword.SecureGateway}} (por ejemplo, entre la app de {{site.data.keyword.Bluemix_notm}} y los servidores de {{site.data.keyword.SecureGateway}}), mientras que TLS para la autenticación de recursos protege la conexión entre {{site.data.keyword.SecureGateway}} y el recurso definido (por ejemplo, entre el cliente de {{site.data.keyword.SecureGateway}} y la base de datos local).

#### Autenticación en la nube/local

Esta opción está disponible si se selecciona TLS o Autenticación mutua para la autenticación de recursos.  El nombre del campo coincidirá con el [tipo de destino](#dest-types) que ha elegido.  Este campo permite que se carguen hasta 6 certificados para validar el certificado del recurso al que se está conectando.  Estos archivos se añadirán a la CA de la conexión con el recurso y deberían contener el certificado o la cadena de certificados que presentará el recurso.

#### Indicador de nombre de servidor (SNI)
Esta opción está disponible si se selecciona TLS o Autenticación mutua para la autenticación de recursos.  Se utiliza para permitir que se especifique un nombre de host independiente al reconocimiento TLS de la conexión del recurso.

### Certificado y clave de cliente
Dónde aparecen los campos Certificado y clave de cliente depende del [tipo de destino](#dest-types) que haya elegido.  En ambas situaciones, el cliente SG utilizará los archivos proporcionados aquí para identificarse a sí mismo para las conexiones TLS.  Si no se carga ningún archivo, los servidores de {{site.data.keyword.SecureGateway}} generarán automáticamente un par autofirmado con un CN de `localhost`.  Para ver instrucciones sobre cómo generar un par de certificado/clave, [pulse aquí](./securegateway_keygen.html).

Para un destino local, aparecerá bajo Autenticación de recursos si se ha seleccionado la opción Autenticación de recursos: Autenticación mutua.  En este caso, el cliente utilizará este par de certificado/clave para su conexión de salida con el recurso definido.  La entidad emisora de certificados (CA) de esta conexión contendrá los certificados proporcionados en el campo [Autenticación de nube/local](#resource-auth).

En el caso de un destino en la nube, aparecerá bajo Autenticación de usuarios si se ha seleccionado un protocolo TLS.  En este caso, el cliente utilizará este par de certificado/clave para establecer los escuchas TLS con el archivo cargado en la [Autenticación de usuarios](#user-auth) en la CA.  

## Configuración de la seguridad de la red
Para impedir que todas las direcciones IP, salvo direcciones específicas, se conecten a su puerto y host de la nube, puede imponer reglas de iptable en su destino local. ![Panel Seguridad de la red](./images/networkSecurity.png?raw=true "Panel Seguridad de la red")

Para imponer reglas iptable, marque el recuadro <b>Restringir el acceso a la nube a este destino con reglas iptable</b> en el panel Seguridad de la red.  Una vez que el recuadro esté marcado, puede empezar a añadir las direcciones IP a las que se debe permitir la conexión.  Si no se especifica ninguna IP, todas las conexiones con este puerto y host de nube se rechazarán mientras el recuadro <b>Restringir el acceso a la nube</b> esté marcado.

<b>Nota</b>: las direcciones IP o puertos especificados deben ser direcciones IP externas que los servidores de {{site.data.keyword.SecureGateway}} verán, no la dirección IP local de la máquina que realiza la solicitud.

### Adición de reglas iptable
Al añadir reglas a iptables, puede proporcionar IP individuales o un rango de IP, junto con un solo puerto o un rango de puertos.  Todos los rangos proporcionados son inclusivos.  En la tabla siguiente se muestran algunos ejemplos, así como la forma en que se resolverá en las iptables:

Direcciones IP | Puertos | Resultados
-- | -- | --
1.2.3.4 | 5000 | Solo se permitirá la IP 1.2.3.4 procedente del puerto 5000.
1.2.3.4-2.3.4.5 | 5000 | Se permitirán todas las IP comprendidas entre 1.2.3.4 y 2.3.4.5 procedentes del puerto 5000.
1.2.3.4 | 5000:10000 | Solo se permitirá la IP 1.2.3.4 procedente de los puertos del 5000 al 10000.
1.2.3.4-2.3.4.5 | 5000:10000 | Se permitirán todas las IP comprendidas entre 1.2.3.4 y 2.3.4.5 procedentes de los puertos del 5000 al 10000.
1.2.3.4 | | Solo se permitirá la IP 1.2.3.4 procedente de cualquier puerto.
| 5000 | Se permitirá cualquier IP procedente del puerto 5000.

También se pueden asociar a una aplicación reglas específicas.  Para obtener más información sobre cómo crear reglas asociadas, consulte [cómo crear reglas de iptable para su app](./iptables.html).

## Configuración de opciones de proxy
Si el destino local se encuentra tras un proxy SOCKS, puede configurar los valores del proxy para el destino en el panel Opciones de proxy. ![Panel Opciones de proxy](./images/proxyOptions.png?raw=true "Panel Opciones de proxy")

Para configurar los valores de proxy, solo tiene que especificar el nombre de host y el puerto en el que escucha el proxy, así como el protocolo SOCKS que se está utilizando (4, 4a, 5).

## Valores de destino
Una vez creado el destino, pulse el icono de valores para ver la siguiente información:

- El ID de destino necesario para utilizar la API.
- <b>Los destinos locales tendrán un puerto y host de nube.  La aplicación necesita esta información para conectarse a su recurso local.</b>
- <b>Los destinos de nube tendrán un puerto de cliente.  Es el puerto en el que escucha el cliente de {{site.data.keyword.SecureGateway}} para conectar la aplicación local al recurso en la nube.</b>
- El puerto y el host de recurso a los que apunta este destino.
- Cuándo se ha creado el destino y el usuario que lo ha creado.
- Cuándo se ha modificado por última vez el destino y el usuario que lo ha modificado.
