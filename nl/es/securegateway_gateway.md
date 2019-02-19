---

copyright:
  years: 2015, 2017
lastupdated: "2017-04-10"

---

# Adición de una pasarela
{: #add-sg-gw}

Una pasarela se puede considerar un método para identificar una determinada red o entorno.  Es lo que utiliza un cliente de Secure Gateway para establecer conectividad con los servidores de Secure Gateway y puede contener varias definiciones de recursos o destinos.

![Panel de control de Secure Gateway](./images/newDashboard.png?raw=true "Panel de control de Secure Gateway")

En el panel de control de Secure Gateway, pulse el botón Añadir pasarela para abrir el panel Añadir pasarela.

![Añadir pasarela](./images/addGateway.png?raw=true "Añadir pasarela")

Lo único que tiene que especificar necesariamente en este panel es el nombre de la pasarela.  De forma predeterminada, tanto `Requerir señal de seguridad` como `Caducidad de la señal` están seleccionados.

Al requerir la señal de seguridad para conectar a los clientes, cada vez que inicie un cliente de Secure Gateway deberá especificar tanto el ID de pasarela como la señal de seguridad.  Si elimina la marca del recuadro `Requerir señal de seguridad`, solo tendrá que especificar el ID de pasarela para que el cliente se pueda conectar.

La fecha de caducidad predeterminada de la señal de seguridad es de 90 días a partir del día en que se crea.  Para cambiar la fecha de caducidad, mantenga marcado el recuadro `Caducidad de la señal` y edite el campo de texto con el número de días en que desea que la señal caduque (el mínimo es 1, el máximo es 365).  Para crear una señal que no caduque, elimine la marca del recuadro `Caducidad de la señal`.  

Para finalizar la creación de la pasarela, pulse Añadir pasarela.  Una vez que se haya creado la pasarela, la página se desplazará automáticamente a la nueva pasarela.

![Nueva pasarela](./images/newGateway.png?raw=true "Nueva pasarela")

Ahora que tiene una pasarela recién creada, puede [conectar su primer cliente](/docs/services/SecureGateway/securegateway_client.html).
