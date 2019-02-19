---

copyright:
  years: 2015, 2018
lastupdated: "2018-08-10"

---

# Requisitos para ejecutar el cliente

## Requisitos del sistema
{: #system}

El cliente de Secure Gateway recibe soporte en los siguientes entornos:

| Nombre | Versiones          |
| ------------- | ----------- |
| Windows Desktop | 7, 8.1, 10 y posteriores |
| Windows Server | 2012 R2 y posteriores |
| Red Hat Linux | 6.5 y posteriores |
| CentOS | 7.2 y posteriores |
| SuSE Linux | 11.0<sup>*</sup> y posteriores |
| Ubuntu Linux | 14.04 (LTS) y posteriores |
| Power Machine | Arquitectura ppc64el |
| Ubuntu Z-Linux | - |
| AIX | 7.1 y posteriores |
| Mac OS X | 10.10 (Yosemite) y posteriores |
| Docker | 1.7.0 y posteriores, todos los sistemas operativos soportados |

<sup> * </sup>- El soporte de SLES 11 solo es con el Service Pack 4

<b>Nota:</b> actualmente solo se da soporte a entornos de 64 bits para la instalación del cliente nativo.

## Requisitos de la red
{: #network}

El cliente de Secure Gateway utiliza el puerto de salida 443 y el puerto 9000 para conectar con el registro npm y el entorno de {{site.data.keyword.Bluemix}}:
- Puerto 443 para la instalación de npm
  - Durante la instalación, el instalador se conectará al registro npm y ejecutará `npm install` para instalar las dependencias necesarias para el cliente Secure Gateway. Antes de la instalación, asegúrese de que la máquina en la que se instalará el cliente se pueda conectar a un sitio web de registro de npm. npm se configura para que utilice el registro público de npm, Inc., que está en https://registry.npmjs.org de forma predeterminada. <br><br>
Si hay un servidor de npm Enterprise en el entorno, coloque en la lista blanca todas las dependencias del cliente de Secure Gateway en el servidor de npm Enterprise. Para ver la lista de dependencias, consulte el archivo `<Installation_directory>\ibm\securegateway\client\package.json`.<br><br>

- Puerto 443 para la autenticación de pasarela


  | Región  | Host  |
  | --  | --  |
  | EE.UU. sur  | sgmanager.ng.bluemix.net  |
  | EE.UU. este  | sgmanager.us-east.bluemix.net  |
  | Reino Unido  | sgmanager.eu-gb.bluemix.net  |
  | Alemania  | sgmanager.eu-de.bluemix.net  |
  | Sídney  | sgmanager.au-syd.bluemix.net  |


- Puerto 9000

  El nodo de la pasarela de SG, que se puede encontrar en la configuración de la pasarela, cada pasarela no estará en el mismo nodo, confirme el nombre de host del nodo cada vez que cree la pasarela


Asegúrese de comprobar o modificar
las reglas de Tabla de IP y cortafuegos adicionales que puedan aplicarse. Si los administradores de red necesitan IP específicas, [póngase en contacto con el equipo de soporte para solicitar estos datos para su entorno](/docs/services/SecureGateway/securegateway_troubleshooting.html#support).


## Determinación de los requisitos de hardware
{: #hardware}

Las especificaciones de la máquina que ejecuta el cliente de Secure Gateway dependen en gran medida del tráfico que pasará a través de cada conexión.  Cada instancia del cliente es un proceso individual que proporciona un máximo de 250 conexiones simultáneas.  Para calcular aproximadamente un máximo promedio al que la máquina debería dar soporte, determine el tamaño medio de una transacción a través del cliente (tanto la solicitud como respuesta) y luego calcúlelo para 250 transacciones simultáneas.  Dado que este número ha combinado tamaños de solicitud y de respuesta, el cliente no debería superar este tamaño de memoria.  Para obtener estimaciones más precisas, se debe utilizar una combinación de tamaños de solicitud y tamaños de respuesta para simular mejor un escenario de transacciones real.

Para ejecutar una sola instancia del cliente de Secure Gateway, recomendamos un mínimo de 2 núcleos y 4 GB de memoria.

Para escenarios de alta disponibilidad que vayan a ejecutar varias instancias del cliente en la misma máquina, recomendamos un mínimo de 4 núcleos y 8 GB de memoria.
