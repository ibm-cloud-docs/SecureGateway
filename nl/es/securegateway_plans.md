---

copyright:
  years: 2015, 2018
lastupdated: "2018-08-10"

---

# Planes de servicio de {{site.data.keyword.SecureGateway}}
{: #secure-gateway-service-plans}

Con el release v1.7.0, se a incorporado un nuevo modelo de precios de plan por niveles que permite escalar {{site.data.keyword.SecureGateway}} para adaptarlo a las necesidades de cualquier empresa.  En la tabla siguiente se muestran las limitaciones asociadas a cada uno de los planes de disponibilidad pública:

![Modelo de plan por niveles](./images/planDetails.png?raw=true "Modelo de plan por niveles")

## Cambio de planes
{: #changing-plans}
Si cambia su plan, el nuevo plan se considerará una actualización o una degradación.  Lo que lo determina es el número predeterminado de pasarelas permitidas por cada plan (por ejemplo, pasar de Professional (5) a Enterprise (25) es una actualización).  Cuando se actualiza, no hay interrupción en el servicio; sin embargo, una degradación actualiza todas las pasarelas para que estén [inactivas](/docs/services/SecureGateway/securegateway_faq.html#faq-states), lo que requiere que reactive sus pasarelas y destinos (hasta el límite del nuevo plan) antes de que se restablezca el servicio.

<b>Nota</b>: cuando se pasa del plan Estándar a cualquiera de los planes nuevos, se considera una degradación.


## Notificación de límite excedido
{: #limit-exceeded-notifications}
Cuando se crea la cantidad máxima permitida de pasarelas/destinos, se muestra un registro con nivel de aviso en el panel de control de la pasarela/destino.

Cuando se alcanza la cantidad de clientes y hay un intento de un nuevo cliente de conectarse a la pasarela, se genera un registro con nivel de error que se muestra en el registro del cliente, y la pasarela echa al nuevo cliente.

Cuando el uso de datos supera el límite de transferencias de datos, se genera un registro con nivel de error que se muestra en el registro del cliente y la pasarela echa al cliente.
