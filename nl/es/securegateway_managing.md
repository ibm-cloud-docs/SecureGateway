---

copyright:
  years: 2015, 2017
lastupdated: "2017-04-10"

---

# Gestión del servicio {{site.data.keyword.SecureGateway}}

## Panel de control de {{site.data.keyword.SecureGateway}}
Desde el panel de control de {{site.data.keyword.SecureGateway}}, puede ver rápidamente los detalles siguientes:

- El número actual de conexiones con sus destinos en todas las pasarelas.
- El flujo de datos de entrada a través de todas las pasarelas durante las últimas 12 horas.
- El flujo de datos de salida a través de todas las pasarelas durante las últimas 12 horas.
- Un gráfico que muestra las 12 últimas horas de uso de cada pasarela.
- Las pasarelas existentes, su estado actual y el número de destinos que contienen.

![Panel de control de {{site.data.keyword.SecureGateway}} con uso](./images/dashboardUsage.png?raw=true "Panel de control de {{site.data.keyword.SecureGateway}} con uso")

### Perfil del servicio
Si pulsa el botón Perfil, verá lo siguiente:

Campo | Descripción
-- | --
Nivel del plan | El plan de servicio seleccionado actualmente.
Límite de pasarelas | El número de pasarelas que puede tener sin que se le facturen pasarelas adicionales.
Límite de clientes | El número de clientes que puede conectar a una pasarela.
Límite de destinos | El número de destinos que puede contener una pasarela.
Límite de datos | La cantidad de transferencia de datos (en GB) a la que da soporte el plan sin que se le facture un exceso de datos.
Excedentes de pasarelas | El número actual de excedentes de pasarela por las que se le está facturando.
Excedentes de datos | El número de excedentes de datos en los que ha incurrido durante el ciclo de facturación del mes actual.

### Panel de información de pasarela
Si pulsa el botón Valores de cualquiera de los mosaicos de pasarela, puede ver:

- La señal de seguridad necesaria para utilizar la API o para conectar un cliente, en función de si la señal se está aplicando en las conexiones de cliente.
- El ID de pasarela necesario para utilizar la API o para conectar un cliente.
- El nodo en el que se ha creado la pasarela.  Es el servidor al que se conectará el cliente.
- Cuándo se ha creado la pasarela y el usuario que la ha creado.
- Cuándo se ha modificado por última vez la pasarela y el usuario que la ha modificado.
- Un enlace para volver a generar el certificado y la clave asociados con la pasarela.  Estos archivos solo se utilizan con destinos antiguos que no contienen su propio par de certificado/clave para la autenticación mutua en el cliente.

Desde el panel de información de la pasarela también se pueden llevar a cabo las tareas siguientes:

- Editar o ver más información acerca de la pasarela y la señal de seguridad.  Se muestra un nuevo panel al seleccionar esta opción.
- Habilitar o inhabilitar la pasarela.
- Suprimir la pasarela.

## Panel de control para una pasarela determinada
Si pulsa en uno de los mosaicos de pasarela, se visualiza el panel de control de dicha pasarela en concreto.  Al igual que sucede con el panel de control principal de {{site.data.keyword.SecureGateway}}, puede ver rápidamente los detalles siguientes:

- El número actual de conexiones con los destinos de esta pasarela.
- El flujo de datos de entrada a través de esta pasarela durante las últimas 12 horas.
- El flujo de datos de salida a través de esta pasarela durante las últimas 12 horas.
- Un gráfico que muestra las 12 últimas horas de uso de cada destino.
- Los destinos existentes, su estado actual y el número actual de conexiones activas.

![Panel de control de una pasarela en concreto](./images/viewGateway.png?raw=true "Panel de control de una pasarela en concreto")

### Panel Información del destino
Si pulsa el botón Valores de cualquiera de los mosaicos de destino, puede ver:

- El ID de destino necesario para utilizar la API.
- Los destinos locales tendrán un puerto y host de nube.  La aplicación necesita esta información para conectarse a su recurso local.
- Los destinos de nube tendrán un puerto de cliente.  Es el puerto en el que escucha el cliente de {{site.data.keyword.SecureGateway}} para conectar la aplicación local al recurso en la nube.
- El puerto y el host de recurso a los que apunta este destino.
- Cuándo se ha creado el destino y el usuario que lo ha creado.
- Cuándo se ha modificado por última vez el destino y el usuario que lo ha modificado.

Desde el panel Información del destino también se pueden llevar a cabo las tareas siguientes:

- Editar o ver más información sobre el destino.  Se muestra un nuevo panel al seleccionar esta opción.
- Habilitar o inhabilitar el destino.
- Suprimir el destino.
