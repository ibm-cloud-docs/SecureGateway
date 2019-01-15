---

copyright:
  years: 2015, 2017
lastupdated: "2017-04-10"

---

# Acerca de {{site.data.keyword.SecureGateway}}

## ¿Cuán seguro es?
{{site.data.keyword.SecureGatewayfull}} mantiene una única conexión de cifrado persistente (TLS v1.2) entre el cliente de {{site.data.keyword.SecureGateway}} (en la red local) y los servidores de {{site.data.keyword.SecureGateway}}.  Con esta conexión bidireccional, podemos transmitir de forma segura datos entre los recursos de la nube y los recursos locales.  Para las conexiones de cualquier extremo (entre recursos de nube y servidores de SG y entre recursos locales y el cliente de SG), el usuario define el protocolo (TCP, TLS, HTTP, HTTPS, UDP) y las medidas de seguridad (autenticación mutua, iptables) que se colocan en ellos.  

## Soporte bidireccional
Al combinar el destino local tradicional con el nuevo destino de la nube, obtenemos soporte bidireccional completo.  El nuevo destino de nube permite que un servicio/aplicación local envíe información/solicitudes a una aplicación en la nube a través del cliente de {{site.data.keyword.SecureGateway}}.  Cuando se crea un nuevo destino de nube, el destino contiene el `host del recurso` (el nombre de host o la IP de la aplicación de nube), el `puerto del recurso` (el puerto en el que la aplicación de nube estará a la escucha) y el `puerto del cliente` (el puerto en el que el cliente de {{site.data.keyword.SecureGateway}} estará a la escucha).  Una vez establecido este destino, el servicio local puede conectarse al puerto de cliente y empezar a enviar datos, que se enviarán a través del cliente, al servidor de {{site.data.keyword.SecureGateway}} y, a continuación, a la aplicación de nube especificada.

El puerto de cliente que se proporciona para el destino debe ser único dentro de la pasarela asociada.  Por ejemplo, una sola pasarela no puede tener dos destinos inversos a la escucha en el puerto 12000, pero dos pasarelas pueden tener cada una su propio destino a la escucha en el puerto 12000.  Sin embargo, en este último caso, si ambas pasares se conectan al mismo cliente, únicamente uno de los destinos podrá escuchar en el puerto 12000.

### Destino en la nube
![Destino en la nube](./images/reverseDestination.png?raw=true "Destino en la nube")

### Destino local
![Destino local](./images/onPremDestination.png?raw=true "Destino local")

## Alta disponibilidad
Para conseguir una alta disponibilidad, cree múltiples conexiones de cliente para el mismo ID de pasarela.  Esto se puede conseguir conectando varios clientes de pasarela única con el mismo ID de pasarela y/o emitiendo varias conexiones en un único cliente de varias pasarelas al mismo ID de pasarela.  Las conexiones se distribuirán entre todos los clientes conectados de forma rotativa.
