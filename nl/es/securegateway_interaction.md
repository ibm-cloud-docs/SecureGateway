---

copyright:
  years: 2015, 2018
lastupdated: "2018-10-04"

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Interacción con el cliente

Hay varias formas de interactuar con el cliente:

- Mediante la línea de mandatos de terminal antes del inicio
- Después del inicio, mediante la línea de mandatos interactiva del cliente
- Después del inicio, mediante la GUI local del cliente

## Configuraciones de inicio
{: #startup}

### Argumentos y opciones de inicio
{: #startup-args}

En la tabla siguiente se describen todas las opciones disponibles que se pueden especificar junto con el mandato de inicio (startup) para el cliente.  Estas opciones permiten la configuración completa del cliente antes del inicio, en lugar de tener que realizar configuraciones individuales una vez se ejecuta el cliente.

| Parámetro y argumentos | Descripción |
| ------------- | ----------- |
| &lt;gateway ID&gt; | Conectar a {{site.data.keyword.Bluemix_notm}} mediante el ID de pasarela especificado |
| -F, -\-aclfile &lt;file&gt; | Access control List file |
| -g, -\-gateway &lt;hostname:port&gt; | Used to manually select a specific gateway destination (advanced use only) |
| -l, -\-loglevel &lt;level&gt; | Change the log level to ERROR, INFO, DEBUG or TRACE |
| -p, -\-logpath &lt;file&gt; | Direct logging to a specific file |
| -t, -\-sectoken &lt;security token&gt; | The security token to use for this gateway connection |
| -P, -\-port &lt;port&gt; | The port for the UI to run on.  Defaults to port 9003 |
| -w, -\-password &lt;password&gt; | The password to protect the UI with.  Defaults to no password |
| -x, -\-proxy &lt;proxy agent&gt; | The proxy for the port 9000 connection |
| -\-noUI | Prevent the UI from starting up automatically |
| -\-allow | Allows all connections to the client. Is overridden by the ACL file, if provided |
| -\-service | After an initial connection, the parent will restart within 60s if all child clients are terminated |

<b>Nota:</b> los distintivos `--service`, `--allow` y `--noUI` deben ser los últimos parámetros de los argumentos de la línea de mandatos.

El caso de uso más básico consiste en iniciar con una sola conexión de cliente con los valores predeterminados:

```
<gateway_id> -t <security_token>
```
{: pre}

Estas opciones se pueden ampliar para dar también soporte a la conexión automática de varios clientes.  Para poder pasar estas opciones a varias conexiones de cliente, se deben pasar utilizando un formato específico.  Los ID de pasarela se pueden pasar de la misma forma que con la conexión de un solo cliente
(con cada ID de pasarela separado por un espacio); sin embargo, el orden de estos ID determina el orden
que deben seguir el resto de los argumentos.  Cuando se pasa cualquiera de los otros argumentos, deben estar separados por `--` para poder seleccionarse correctamente.  Si no se pasa nada
para un determinado distintivo, se presupondrá que no se aplicarán a ninguno de los clientes.

Si no se especifican suficientes argumentos para rellenar todos los ID de pasarela, se aplicarán
en orden hasta que no haya más argumentos.  Por ejemplo, si se pasan dos ID de pasarela
y se pasa una sola señal de seguridad, la señal se aplicará al primer ID de pasarela y no
al segundo.  

Si se especifican ID de pasarela que necesitan distintos argumentos, se debe designar la palabra clave
`none` en lugar de cualquier argumento concreto que una pasarela no está
imponiendo o proporcionando.  Por ejemplo, si se pasan tres ID de pasarela y el usuario desea especificar
un nivel de rastreo para la primera y la tercera, el argumento tendrá el aspecto siguiente: `--loglevel
DEBUG--none--TRACE`.  En este caso, none tendrá como valor predeterminado INFO.

### Ejemplos de argumentos de inicio
{: #startup-examples}

Una sola conexión de cliente, nivel de registro personalizado, sin IU:

```
node lib/secgwclient.js <gateway_id> -t <security_token> -l <loglevel> --noUI
```
{: pre}

Varias conexiones de cliente, opciones predeterminadas:

```
node lib/secgwclient.js <gateway_id_1> <gateway_id_2> -t <security_token_1>--<security_token_2>
```
{: pre}

Varias conexiones de cliente, todos los valores personalizados:

```
node lib/secgwclient.js <myGatewayID_1> <myGatewayID_2> -t none--<token for gateway 2> -l DEBUG--TRACE -p <full path to log file for gateway 1>--<full path to log file for gateway 2> -F <full path to ACL file for gateway 1>
```
{: pre}

Volver a [Iniciación - Adición de un cliente](/docs/services/SecureGateway/securegateway_client.html).

## Configuración interactiva
{: #interactive}

### Mandatos interactivos
{: #interactive-commands}

El cliente de {{site.data.keyword.SecureGateway}} tiene una
interfaz de línea de mandatos (cli) y un indicador de shell para que el control y la configuración sean sencillos. El entorno interactivo
da soporte a un conjunto más amplio de funciones que los argumentos de la línea de mandatos para facilitar
el control interactivo en el cliente.

| Mandatos interactivos | Descripción       |
| ------------- | ----------- |
| A, acl allow &lt;hostname:port&gt; &lt;worker ID&gt; | Lista de control de accesos permitidos |
| D, acl deny &lt;hostname:port&gt; &lt;worker ID&gt; | Lista de control de accesos denegados |
| N, no acl &lt;hostname:port&gt; &lt;worker ID&gt; | Eliminar una entrada de la lista de control de accesos |
| S, show acl &lt;worker ID&gt; | Mostrar todas las entradas de la lista de control de accesos |
| F, acl file &lt;file&gt; &lt;worker ID&gt; | Archivo de lista de control de accesos |
| C, displayconfig &lt;worker ID&gt; | Muestra la configuración actual de {{site.data.keyword.SecureGateway}} si está disponible. |
| a, authorize &lt;worker ID&gt; | Conmutar la sustitución del parámetro rejectUnauthorized para conexiones TLS de salida para el nodo trabajador especificado |
| t, sectoken &lt;security token&gt; | La señal de seguridad que se debe utilizar para la siguiente conexión de pasarela |
| c, connect &lt;gateway ID&gt; | Conectar a {{site.data.keyword.Bluemix_notm}} mediante el ID de pasarela especificado |
| l, loglevel &lt;level&gt; &lt;worker ID&gt; | Cambiar el nivel de registro a ERROR, INFO, DEBUG o TRACE |
| p, logpath &lt;file&gt; &lt;worker ID&gt; | Registro directo en un archivo específico |
| r, reverse &lt;worker ID&gt; | Listar los puertos que está escuchando en este momento el cliente para destinos inversos |
| k, kill &lt;worker ID&gt;  | Terminar el nodo trabajador especificado |
| e, select &lt;worker ID&gt;  | Especifica un nodo trabajador en el que realizar los mandatos, a menos que se especifique lo contrario |
| d, deselect | Deselecciona el nodo trabajador especificado anteriormente.  Emita el mandato seleccionado para especificar
otro |
| w, password &lt;old password&gt; &lt;new password&gt; | Establecer la contraseña de IU.  Si &lt;new password&gt; está en blanco, no se impone ninguna contraseña. Hay que especificar &lt;old password&gt; al actualizar la contraseña. Las contraseñas deben contener solamente letras |
| P, port &lt;new port&gt; | Cambiar el puerto en el que está a la escucha la interfaz de usuario. |
| u, uistart &lt;initial password&gt; &lt;port&gt; | Inicia la IU en localhost:&lt;port&gt;/ashboard. Si &lt;initial password&gt; está en blanco y no se ha establecido ninguna otra contraseña para la sesión, no se impondrá ninguna contraseña de interfaz de usuario. Si &lt;port&gt; está en blanco, se podrá acceder a la interfaz de usuario en 9003 |
| U, uistop | Cierra la interfaz de usuario asociada a esta sesión de cliente. Solo se podrá acceder a la sesión mediante la CLI hasta que se inicie manualmente una nueva IU |
| R, revoke | Borrar todas las autorizaciones de interfaz de usuario asociadas a esta sesión de cliente |
| q, quit | Desconectar y salir |
| s, status &lt;worker ID&gt;  | Mostrar los detalles de estado del túnel y sus conexiones |
| L, list | Muestra una lista de los nodos trabajadores asociados actualmente |

<b>Nota:</b> si se ha especificado una conexión con el mandato `select` y se llama a otro mandato sin proporcionar un ID de nodo trabajador, el mandato intentará ejecutarse en la conexión especificada por `select`.

Para obtener más información sobre cómo configurar la lista de control de accesos, [pulse aquí](/docs/services/SecureGateway/securegateway_acl.html).

Volver a [Iniciación - Adición de un cliente](/docs/services/SecureGateway/securegateway_client.html).

## Interfaz de usuario de cliente
{: #ui}

<b>Nota:</b> la IU de cliente no recibe soporte cuando se utiliza Docker en Windows o MacOS.

La interfaz de usuario de cliente proporciona una interfaz web local para que el usuario interactúe con el cliente de {{site.data.keyword.SecureGateway}} en lugar de la CLI.  De forma predeterminada, esta interfaz de usuario está disponible en `localhost:9003/dashboard`. La interfaz de usuario se divide en las páginas siguientes:

### Conectar
{: #ui-connect}

Es la página inicial para la interfaz de usuario, donde el usuario puede especificar un ID de pasarela y una señal de seguridad para conectarse al primer cliente.

### Iniciar sesión
{: #ui-login}

Esta página se mostrará si la interfaz de usuario ha sido protegida mediante contraseña.  Si se accede a esta página sin que se haya impuesto ninguna contraseña, renueve la página para ser redirigido.

### Panel de control
{: #ui-dashboard}

Esta es la página principal una vez que se ha conectado un cliente.  Desde aquí puede acceder a la página Ver registros, a la página Lista de control de accesos y a la página Información de conexión.  En la parte inferior, también puede optar por desconectar uno o muchos de los clientes conectados.  En la parte superior de la página se muestra el cliente seleccionado actualmente, así como una opción para conectar más clientes.

### Ver registros
{: #ui-logs}

Esta página le permitirá ver los registros que está generando el cliente seleccionado (se muestra en la parte superior derecha de la página).  Los registros visualizados pueden filtrarse mediante los recuadros de selección debajo de los registros.

### Lista de control de accesos
{: #ui-acl}

Esta página le permitirá manipular la lista de control de accesos para el cliente seleccionado (que se muestra en la parte superior derecha de la página).  Se pueden añadir reglas individualmente a las tablas de permitir o denegar que se puedan cargar tablas o un archivo en la parte inferior de la página.

### Información de conexión
{: #ui-info}

Esta página mostrará información de la conexión actual para el cliente seleccionado (que se muestra en la parte superior derecha de la página).  La información como la descripción de contraseña, el número de conexiones actuales y los escuchas de destino inverso puede verse aquí.

Volver a [Iniciación - Adición de un cliente](/docs/services/SecureGateway/securegateway_client.html).

## Terminación de un cliente remoto
{: #remote}

Si se ha proporcionado un ID a un cliente, se puede terminar de forma remota a través de la IU de SG o a través de la API de SG.  Si termina un cliente que se ejecuta como un servicio, el cliente se reiniciará y obtendrá un nuevo ID de cliente; sin embargo, si el servicio tiene varios clientes conectados, el cliente terminado no se reiniciará hasta que todos los clientes restantes se hayan terminado.

## Limitaciones
{: #limits}

### Limitaciones de conexión
{: #limits-conn}

La pasarela de SG solo puede gestionar 250 conexiones simultáneas. Si el número de solicitudes simultáneas sobrepasa el límite, puede dar lugar a que se rechacen los intentos de conexión y que eso lleve a un estado de latencia. Una forma fácil de solucionar esto es utilizar la agrupación de conexiones en la app de llamada. Tenga en cuenta que el límite de 250 conexiones simultáneas se encuentra en la pasarela, no en el cliente ni en el destino. Este límite se compartirá entre todos los clientes y destinos de la pasarela.

### Limitaciones de cliente de DataPower
{: #limits-datapower}

El cliente de {{site.data.keyword.SecureGateway}} DataPower está en el proceso de actualización para ajustarse a las funciones del resto de nuestros clientes.  Actualmente tiene las limitaciones siguientes:

- ACL tomará como valor predeterminado ALLOW ALL
- No se admite la habilitación o inhabilitación de pasarelas o destinos desde la interfaz de usuario de {{site.data.keyword.SecureGateway}}. Sin embargo, la opción Estado administrativo de la interfaz de usuario de DataPower funciona como un conmutador de encendido/apagado para un determinado cliente.
- No se admite el sondeo de estado de conexión para actualizaciones de estado de pasarela conectada y desconectada en tiempo real.
- No se admiten cadenas de certificados completos con TLS del lado de destino antes de DataPower versión 7.5.1.0.
- Los destinos de nube no reciben soporte antes de DataPower versión 7.5.1.0.
- El nivel de registro no se puede cambiar por nivel TRACE
