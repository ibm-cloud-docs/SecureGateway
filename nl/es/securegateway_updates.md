---

copyright:
  years: 2015, 2017
lastupdated: "2017-04-25"

---

# Registro de cambios de {{site.data.keyword.SecureGateway}}
{: #secure-gateway-change-log}

## v1.7.0 Fixpack 1
{: #v171fp1}

### Arreglos
{: #v171fp1-fixes}

- Resuelve el error `EADDRINUSE` que se genera cuando los escuchas no ejecutan una eliminación correctamente cuando se suprime un destino.
- Resuelve el problema por el que las instancias de servicio enlazadas a una aplicación no se pueden suprimir.

## v1.7.0
{: #v170}

### Características
{: #v170-features}

- Incorpora nuevos planes de servicio: Essentials, Professional y Enterprise.
- Ahora el cliente de Secure Gateway admite el proxy SOCKS entre sí mismo y el destino local.

## v1.6.0 Fixpack 1
{: #v160fp1}

### Arreglos
{: #v160fp1-fixes}

- Corrige el problema por el que la desconexión de varios clientes da lugar a procesos huérfanos de la matriz de clientes conectados.
- Ahora se limpian las conexiones huérfanas que hacían que se colocaran en pausa escuchas de forma incorrecta.

## v1.6.0
{: #v160}

### Características
{: #v160-features}

- Ahora la lista de control de accesos da soporte al direccionamiento de vía de acceso específico para solicitudes HTTP/S.
- Ahora los destinos dan soporte a indicadores de nombre de servidor para conexiones TLS.

## v1.5.1
{: #v151}

### Características
{: #v151-features}

- Se ha añadido un asistente de destinos para la creación de destinos.
- Ahora las pasarelas permiten cargar un par de certificado/clave personalizado.
- Ahora los servicios están limitados a 50 pasarelas y 50 destinos por pasarela.
- Ahora los destinos/pasarelas/servicios se pueden exportar/importar.
- Se pueden ejecutar pruebas de latencia entre el cliente y el servidor desde la interfaz de usuario de Secure Gateway.

## v1.5.0 Fixpack 1
{: #v150fp1}

### Arreglos
{: #v150fp1-fixes}

- Limpia los procesos de túnel huérfanos tras la desconexión de la red.
- Resuelve la asignación de puertos duplicados causada por la supresión errónea de destinos.
- Resuelve el problema de HTTP/S con una codificación que no es UTF8.
- Resuelve los manejadores IPC que faltan en los procesos de sustitución.

## v1.5.0
{: #v150}

### Características
{: #v150-features}

- Ahora el cliente tiene una interfaz de usuario interactiva local.
- Las conexiones UDP ahora están soportadas.
- Se ha reducido sustancialmente el tamaño de la memoria del cliente.
- Ya no se da soporte a la modalidad de cliente único; la opción de varios clientes es la predeterminada y resulta transparente para un cliente de una sola pasarela.
- La lista de control de accesos se sincroniza entre los clientes conectados a la misma pasarela.

## v1.4.2
{: #v142}

### Características
{: #v142-features}

- Ahora se asigna a los clientes un ID al conectar con el servidor de SG.
- Ahora los clientes se pueden terminar de forma remota mediante la API o la interfaz de usuario de Secure Gateway.
- El estado de conexión de un cliente se puede comprobar mediante la API.
- Ahora TLS del lado de destino admite la autenticación mutua.
- Los destinos de nube admiten los protocolos de autenticación mutua.
