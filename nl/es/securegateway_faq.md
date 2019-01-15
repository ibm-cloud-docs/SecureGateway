---

copyright:
  years: 2015, 2018
lastupdated: "2018-11-08"

---

# Preguntas frecuentes

## Capa del modelo OSI
{: #osi}

### Pregunta
¿Qué capa del modelo OSI representa el servicio Secure Gateway?

### Respuesta
El servicio Secure Gateway representa la capa 4 del modelo OSI.

## Soporte de versión de TLS
{: #tls}

### Pregunta
¿A qué versión de TLS da soporte el servicio Secure Gateway?

### Respuesta
El servicio Secure Gateway da soporte a TLS versión 1.2.

## ¿Por qué debería inhabilitar mi pasarela o destino?
{: #disabled}

### Pregunta

¿Por qué desearía inhabilitar una pasarela o un destino?

### Respuesta
Es posible que desee inhabilitar un destino o una pasarela por una de las siguientes razones:

- Crea un destino, pero no ha configurado la seguridad.  En este caso, puede inhabilitar el destino hasta que se haya configurado la seguridad.
- No desea que el servicio esté disponible para los usuarios porque está realizando algunas actualizaciones en el servicio.  En este caso, puede inhabilitar temporalmente las pasarelas necesarias y esperar a que se actualice el servicio.
- Ha configurado todas las pasarelas y destinos en la parte frontal, pero el programa de fondo aún se está creando.  En este caso, inhabilitaría las pasarelas o destinos hasta que finalice la creación del programa de fondo.

Para obtener más información cómo inhabilitar una pasarela o un destino, consulte [cómo gestionar la instancia del servicio Secure Gateway](./securegateway_managing.html).

## ¿Cuál es el enfoque recomendado para la automatización de la creación en varios espacios?
{: #automation-spaces}

### Pregunta
Un entorno de cliente tiene una organización y tres espacios.  Un espacio es para desarrollo, otro para transferencia y el último para producción.  ¿Debería el cliente crear una sola instancia de Secure Gateway o varias (por ejemplo, una para cada espacio)?  Si el cliente puede crear varias pasarelas, ¿hay alguna consideración a tener en cuenta para reutilizar una aplicación Node.js a fin de crear una pasarela y un destino en cada espacio?

### Respuesta

- Puede crear una sola instancia de Secure Gateway para los tres espacios.  Sin embargo, debe recordar las [limitaciones para su plan específico](./securegateway_plans.html) de pasarela y de destino.
- No hay consideraciones adicionales a tener en cuenta para reutilizar una aplicación Node.js, ya que Secure Gateway no necesita enlaces de servicio.


## ¿Cuál es el enfoque recomendado para la automatización de la creación en varias organizaciones?
{: #automation-orgs}

### Pregunta
Un entorno de cliente tiene tres organizaciones: una para desarrollo, una para transferencia y otra para producción.  ¿Se necesita una instancia de servicio de Secure Gateway para cada organización y está la configuración disponible para todos los espacios de dicha organización?

### Respuesta

- No es necesario tener una instancia de servicio de Secure Gateway en cada organización. Puede tener una instancia en una organización y utilizar las pasarelas de dicha instancia desde todos los demás entornos.  Sin embargo, con esta configuración debe recordar las [limitaciones para su plan específico](./securegateway_plans.html) de pasarela y de destino.
- Puede tener una instancia del servicio Secure Gateway en cada organización y la configuración estará disponible para todos los espacios.

## ¿Es necesario que mi app esté en el mismo espacio?
{: #app-space}

### Pregunta
¿Tiene que ejecutar la app Node.js en el mismo espacio de {{site.data.keyword.Bluemix_notm}} que el servicio Secure Gateway?

### Respuesta
No, no es necesario que ejecute la app en el mismo espacio de {{site.data.keyword.Bluemix_notm}} que el servicio Secure Gateway.

## ¿Puedo obtener los registros del servidor de Secure Gateway?
{: #server-logs}

### Pregunta
¿Puede recuperar registros de errores para el servidor de Secure Gateway?

### Respuesta
Los registros de errores del servidor no se pueden recuperar.  Solo se pueden ver los errores que se producen en el momento de la solicitud.

## ¿Cuáles son los estados funcionales de Secure Gateway?
{: #states}

### Pregunta
¿Cuáles son los diferentes estados del ciclo de vida de las pasarelas y de los destinos?

### Respuesta

#### Estado no funcional
El release 1.7.0 ha incorporado un nuevo modelo de precios del plan por niveles. Con este modelo se incorpora la posibilidad de marcar tanto las pasarelas como los destinos como 'Activos' o 'Inactivos'. Parte de la nueva estructura de facturación del plan factura al usuario según el número de pasarelas y destinos que tienen.

- Los elementos inactivos no se facturan
- Los destinos inactivos no tienen un puerto de nube suministrado.
- Todos los destinos de una pasarela inactiva también estarán inactivos y no se podrán volver a activar hasta que su pasarela también esté activa.
- Los elementos inactivos se consideran no funcionales. Los elementos inactivos no pueden estar en ninguno de los estados funcionales.

#### Estados funcionales
Cuando una pasarela o un destino esté marcado como activo, se facturará. Los estados activos para pasarelas y destinos son los siguientes:

- Habilitado: la pasarela o el destino está listo para ser utilizado en circunstancias normales.
- Inhabilitado: la pasarela o el destino se ha inhabilitado.
    - Pasarelas: los clientes no podrán hacer fluir datos a través de la pasarela inhabilitada.
    - Destinos: los clientes no podrán crear conexiones con estos destinos inhabilitados.

## ¿Cómo puedo saber las actividades de datos en el cliente de Secure Gateway?
{: #data-size}

### Pregunta
¿Cómo puedo saber las actividades de datos realizadas a través del cliente de Secure Gateway?

### Respuesta
En el cliente de SecureGateway, cambie el nivel de registro por TRACE. Se mostrará la siguiente información después de que se envíen solicitudes.

Tamaño de los datos enviados desde la aplicación de solicitud:
```
[TRACE] Connection #<connection ID> transmitted data: <size> bytes
```

Tamaño de los datos enviados desde el destino:
```
[TRACE] Connection #<connection ID> received data: <size> bytes
```

## ¿Cómo se obtiene el número de conexión en tiempo real en Secure Gateway?
{: #connect-num}

### Pregunta
¿Cómo se obtiene la información de conexión en tiempo real, el tamaño de los datos enviados y recibidos desde el cliente de Secure Gateway?

### Respuesta

- En la interfaz de mandatos interactiva del cliente de Secure Gateway, escriba `s` para ver detalles del estado de las conexiones. 
- En la IU del cliente de Secure Gateway, pulse el menú Información de conexión

Se mostrarán las siguientes estadísticas de conexión: 
- Tamaño total de los datos transmitidos.
- Tamaño total de los datos recibidos.
- El número total de conexiones.
- Conexiones activas en tiempo real.

Nota: el número de conexiones actuales en la interfaz de usuario de Secure Gateway no se representa en tiempo real. Utilice los métodos anteriores en el cliente de Secure Gateway para recuperar la información de conexión en tiempo real.

## ¿Cuáles son las configuraciones recomendadas para hacer que mis conexiones sean más seguras?
{: #secure-app}

### Pregunta
¿Cuáles son las configuraciones recomendadas para hacer que mis conexiones sean más seguras?

### Respuesta

#### Utilizar autenticación mutua
El hecho de habilitar la autenticación mutua en ambos lados de los destinos locales hace que Secure Gateway sea más segura. En el lado de autenticación de usuario, habilite la autenticación mutua para restringir el acceso al nodo de nube de Secure Gateway mediante la autenticación utilizando un certificado de cliente cuando la solicitud se realice sobre TLS/HTTPS. En el lado de la autenticación de recursos, habilite la autenticación mutua para proporcionar la credencial adecuada al conectar con el punto final de destino y asegure el acceso seguro/cifrado al recurso local. Consulte [Configuración de la autenticación mutua](./securegateway_destination.html#mutual-auth) y [Autenticación mutua de TLS de Node.js](./securegateway_tls-ma.html#node-js-tls-mutual-authentication) para obtener más información.

#### Configurar reglas de IPtable (para destino local)
El puerto y el host de nube de Secure Gateway de un destino local están en el espacio público; por lo tanto, todo el mundo tiene acceso de forma predeterminada.
Para controlar el tráfico de acceso en Secure Gateway, defina reglas de iptable de modo que solo permitan el acceso mediante un rango específico de IP y puertos para proteger los recursos locales. Consulte [Reglas de IPtable](./securegateway_destination.html#configuring-network-security) para obtener más información sobre cómo configurar las reglas de iptable en Secure Gateway.

#### Configurar lista de control de accesos (para destino local)
La configuración del soporte de lista de control de accesos para permitir o restringir el acceso a los recursos locales haría que los destinos locales fueran más seguros mediante la especificación del acceso en el host y puerto de destino específicos. Se recomienda definir las rutas HTTP/S permitidas o restringidas en las entradas de ACL, así como mejorar la seguridad del destino local. Consulte [Lista de control de accesos](./securegateway_acl.html#access-control-list) y [Control de rutas HTTP/S mediante la ACL](./securegateway_acl.html#routes) para obtener más información.

#### Establecer la contraseña en la interfaz de usuario del cliente de Secure Gateway
Se recomienda establecer la contraseña de la interfaz de usuario para restringir el acceso a la interfaz de usuario del cliente de Secure Gateway. Consulte [Interactuación con el cliente](./securegateway_interaction.html#interacting-with-the-client) para ver más información sobre cómo definir la contraseña mediante la configuración de inicio o de mandatos interactivos en la línea de mandatos de terminal del cliente de Secure Gateway.
