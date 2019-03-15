---

copyright:
  years: 2015, 2019
lastupdated: "2019-01-16"

---

# Preguntas frecuentes
{: #sg-faq}

## Capa del modelo OSI
{: #faq-osi}

### Pregunta
{: #osi-question}
¿Qué capa del modelo OSI representa el servicio Secure Gateway?

### Respuesta
{: #osi-answer}
El servicio Secure Gateway representa la capa 4 del modelo OSI.

## Soporte de versión de TLS
{: #faq-tls}

### Pregunta
{: #tls-question}
¿A qué versión de TLS da soporte el servicio Secure Gateway?

### Respuesta
{: #tls-answer}
El servicio Secure Gateway da soporte a TLS versión 1.2.

## ¿Por qué debería inhabilitar mi pasarela o destino?
{: #faq-disabled}

### Pregunta
{: #disabled-question}

¿Por qué desearía inhabilitar una pasarela o un destino?

### Respuesta
{: #disabled-answer}
Es posible que desee inhabilitar un destino o una pasarela por una de las siguientes razones:

- Crea un destino, pero no ha configurado la seguridad.  En este caso, puede inhabilitar el destino hasta que se haya configurado la seguridad.
- No desea que el servicio esté disponible para los usuarios porque está realizando algunas actualizaciones en el servicio.  En este caso, puede inhabilitar temporalmente las pasarelas necesarias y esperar a que se actualice el servicio.
- Ha configurado todas las pasarelas y destinos en la parte frontal, pero el programa de fondo aún se está creando.  En este caso, inhabilitaría las pasarelas o destinos hasta que finalice la creación del programa de fondo.

Para obtener más información cómo inhabilitar una pasarela o un destino, consulte [cómo gestionar la instancia del servicio Secure Gateway](/docs/services/SecureGateway/securegateway_managing.html).

## ¿Cuál es el enfoque recomendado para la automatización de la creación en varios espacios?
{: #faq-automation-spaces}

### Pregunta
{: #automation-spaces-question}
Un entorno de cliente tiene una organización y tres espacios.  Un espacio es para desarrollo, otro para transferencia y el último para producción.  ¿Debería el cliente crear una sola instancia de Secure Gateway o varias (por ejemplo, una para cada espacio)?  Si el cliente puede crear varias pasarelas, ¿hay alguna consideración a tener en cuenta para reutilizar una aplicación Node.js a fin de crear una pasarela y un destino en cada espacio?

### Respuesta
{: #automation-spaces-answer}

- Puede crear una sola instancia de Secure Gateway para los tres espacios.  Sin embargo, debe recordar las [limitaciones para su plan específico](/docs/services/SecureGateway/securegateway_plans.html) de pasarela y de destino.
- No hay consideraciones adicionales a tener en cuenta para reutilizar una aplicación Node.js, ya que Secure Gateway no necesita enlaces de servicio.


## ¿Cuál es el enfoque recomendado para la automatización de la creación en varias organizaciones?
{: #faq-automation-orgs}

### Pregunta
{: #automation-orgs-question}
Un entorno de cliente tiene tres organizaciones: una para desarrollo, una para transferencia y otra para producción.  ¿Se necesita una instancia de servicio de Secure Gateway para cada organización y está la configuración disponible para todos los espacios de dicha organización?

### Respuesta
{: #automation-orgs-answer}

- No es necesario tener una instancia de servicio de Secure Gateway en cada organización. Puede tener una instancia en una organización y utilizar las pasarelas de dicha instancia desde todos los demás entornos.  Sin embargo, con esta configuración debe recordar las [limitaciones para su plan específico](/docs/services/SecureGateway/securegateway_plans.html) de pasarela y de destino.
- Puede tener una instancia del servicio Secure Gateway en cada organización y la configuración estará disponible para todos los espacios.

## ¿Es necesario que mi app esté en el mismo espacio?
{: #faq-app-space}

### Pregunta
{: #app-space-question}
¿Tengo que ejecutar la app Node.js en el mismo espacio de {{site.data.keyword.Bluemix_notm}} que el servicio Secure Gateway?

### Respuesta
{: #app-space-answer}
No, no es necesario que ejecute la app en el mismo espacio de {{site.data.keyword.Bluemix_notm}} que el servicio Secure Gateway.

## ¿Puedo obtener los registros del servidor de Secure Gateway?
{: #faq-server-logs}

### Pregunta
{: #server-logs-question}
¿Puedo recuperar registros de nivel de error para el servidor de Secure Gateway?

### Respuesta
{: #server-logs-answer}
Los registros de nivel de error del servidor no se pueden recuperar.  Solo se pueden ver los errores que se producen en el momento de la solicitud.

## ¿Cuáles son los estados funcionales de Secure Gateway?
{: #faq-states}

### Pregunta
{: #states-question}
¿Cuáles son los diferentes estados del ciclo de vida de las pasarelas y de los destinos?

### Respuesta
{: #states-answer}

#### Estado no funcional
{: #states-answer-non-functional}
El release 1.7.0 ha incorporado un nuevo modelo de precios del plan por niveles. Con este modelo se incorpora la posibilidad de marcar tanto las pasarelas como los destinos como 'Activos' o 'Inactivos'. Parte de la nueva estructura de facturación del plan factura al usuario según el número de pasarelas y destinos que tienen.

- Los elementos inactivos no se facturan
- Los destinos inactivos no tienen un puerto de nube suministrado.
- Todos los destinos de una pasarela inactiva también estarán inactivos y no se podrán volver a activar hasta que su pasarela también esté activa.
- Los elementos inactivos se consideran no funcionales. Los elementos inactivos no pueden estar en ninguno de los estados funcionales.

#### Estados funcionales
{: #states-answer-functional}
Cuando una pasarela o un destino esté marcado como activo, se facturará. Los estados activos para pasarelas y destinos son los siguientes:

- Habilitado: la pasarela o el destino está listo para ser utilizado en circunstancias normales.
- Inhabilitado: la pasarela o el destino se ha inhabilitado.
    - Pasarelas: los clientes no podrán hacer fluir datos a través de la pasarela inhabilitada.
    - Destinos: los clientes no podrán crear conexiones con estos destinos inhabilitados.

## ¿Cómo puedo saber las actividades de datos en el cliente de Secure Gateway?
{: #faq-data-size}

### Pregunta
{: #data-size-question}
¿Cómo puedo saber las actividades de datos realizadas a través del cliente de Secure Gateway?

### Respuesta
{: #data-size-answer}
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
{: #faq-connect-num}

### Pregunta
{: #connect-num-question}
¿Cómo se obtiene la información sobre conexiones, como la cantidad de conexiones en tiempo real y el tamaño de los datos enviados y recibidos desde el cliente de Secure Gateway?

### Respuesta
{: #connect-num-answer}

- En la interfaz de mandatos interactiva del cliente de Secure Gateway, escriba `s` para ver detalles del estado de las conexiones. 
- En la IU del cliente de Secure Gateway, pulse el menú Información de conexión

Se mostrarán las siguientes estadísticas de conexión: 
- Tamaño total de los datos transmitidos.
- Tamaño total de los datos recibidos.
- La cantidad total de conexiones.
- Conexiones activas en tiempo real.

Nota: esta información sobre conexiones es a nivel de cliente, no a nivel de pasarela. Si necesita información sobre conexiones a nivel de pasarela, compruebe cada cliente que se haya conectado a la pasarela.

## ¿Cuáles son las configuraciones recomendadas para hacer que mis conexiones sean más seguras?
{: #faq-secure-app}

### Pregunta
{: #secure-app-question}
¿Cuáles son las configuraciones recomendadas para hacer que mis conexiones sean más seguras?

### Respuesta
{: #secure-app-answer}

#### Utilizar autenticación mutua
{: #secure-app-answer-ma}
El hecho de habilitar la autenticación mutua en ambos lados de los destinos locales hace que Secure Gateway sea más segura. En el lado de autenticación de usuario, habilite la autenticación mutua para restringir el acceso al nodo de nube de Secure Gateway mediante la autenticación utilizando un certificado de cliente cuando la solicitud se realice sobre TLS/HTTPS. En el lado de la autenticación de recursos, habilite la autenticación mutua para proporcionar la credencial adecuada al conectar con el punto final de destino y asegure el acceso seguro/cifrado al recurso local. Consulte [Configuración de la autenticación mutua](/docs/services/SecureGateway/securegateway_destination.html#dest-mutual-auth) y [Autenticación mutua de TLS de Node.js](/docs/services/SecureGateway/securegateway_tls-ma.html#nodejs-tls-ma) para obtener más información.

#### Configurar reglas de IPtable (para destino local)
{: #secure-app-answer-iptables}
El puerto y el host de nube de Secure Gateway de un destino local están en el espacio público; por lo tanto, todo el mundo tiene acceso de forma predeterminada.
Para controlar el tráfico de acceso en Secure Gateway, defina reglas de iptables de modo que solo permitan el acceso mediante un rango específico de IP y puertos para proteger los recursos locales. Consulte [Reglas de IPtable](/docs/services/SecureGateway/securegateway_destination.html#dest-network-security) para obtener más información sobre cómo configurar las reglas de iptables en Secure Gateway.

#### Configurar lista de control de accesos (para destino local)
{: #secure-app-answer-acl}
La configuración del soporte de lista de control de accesos para permitir o restringir el acceso a los recursos locales haría que los destinos locales fueran más seguros mediante la especificación del acceso en el host y puerto de destino específicos. Se recomienda definir las rutas HTTP/S permitidas o restringidas en las entradas de ACL, así como mejorar la seguridad del destino local. Consulte [Lista de control de accesos](/docs/services/SecureGateway/securegateway_acl.html#acl) y [Control de rutas HTTP/S mediante la ACL](/docs/services/SecureGateway/securegateway_acl.html#acl-route-control) para obtener más información.

#### Establecer la contraseña en la interfaz de usuario del cliente de Secure Gateway
{: #secure-app-answer-ui-pw}
Se recomienda establecer la contraseña de la interfaz de usuario para restringir el acceso a la interfaz de usuario del cliente de Secure Gateway. Consulte [Interactuación con el cliente](/docs/services/SecureGateway/securegateway_interaction.html#client-interacting) para ver más información sobre cómo definir la contraseña mediante la configuración de inicio o de mandatos interactivos en la línea de mandatos de terminal del cliente de Secure Gateway.

## ¿Qué es la migración de pasarela? ¿Por qué cambia el dominio después de diciembre de 2018?
{: #faq-gateway-migration}

### Pregunta
{: #gateway-migration-question}
Después del mantenimiento de diciembre de 2018, aparece un botón Migrar en el panel de la pasarela. ¿Para qué sirve este botón? ¿Por qué cambia el dominio?

### Respuesta
{: #gateway-migration-answer}

Después del mantenimiento de diciembre de 2018, se cambia el nombre del host de la nube de Secure Gateway por `securegateway.appdomain.cloud` en lugar de `integration.ibmcloud.com` y se utiliza `securegateway.cloud.ibm.com` para la autenticación de pasarela en lugar de `bluemix.net`. Por motivos de compatibilidad con versiones anteriores, la pasarela existente seguirá utilizando el dominio antiguo hasta que se migre la pasarela y el cliente SG v180fp9 y anteriores seguirán utilizando `bluemix.net` para la autenticación de pasarela.

Después de la migración, el host de nube de los destinos locales se modificará para utilizar el nuevo dominio; los usuarios y las aplicaciones tendrá que actualizarse para enviar la solicitud al nuevo host de nube.

Actualmente la migración no es obligatoria y no hay fecha exacta sobre cuándo se dejará de dar soporte al dominio antiguo; una vez determinada, se notificará a los clientes que sigan utilizando el nombre de dominio antiguo.

## ¿Dónde puedo recibir notificaciones?
{: #faq-notification}

### Pregunta
{: #notification-question}
¿Dónde puedo recibir notificaciones de Secure Gateway, especialmente en lo referente a interrupciones por mantenimiento?

### Respuesta
{: #notification-answer}

Puede recibir notificaciones a través de nuestra [página de estado](https://console.bluemix.net/status); busque `Secure Gateway` en dicha página.

Si el cliente Secure Gateway se desconecta de forma inesperada, vaya a la página de estado para ver si se ha producido una interrupción por mantenimiento en este momento.
