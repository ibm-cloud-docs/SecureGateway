---

copyright:
  years: 2015, 2019
lastupdated: "2019-04-09"

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Reglas de iptable
{: #iptables-rules}

Para permitir la imposición de reglas de iptables en el destino, debe tener la opción `Restringir acceso a la red` marcada en el panel de seguridad de red del destino.  En este punto, puede añadir las reglas que desea imponer, como por ejemplo 192.0.0.1 9000 (una sola IP y un solo puerto), 192.0.0.1-192.0.0.5 5000:5005 (rango de IP y rango de puertos) o cualquier combinación de estas reglas. Consulte [Configuración de la seguridad de la red](/docs/services/SecureGateway?topic=securegateway-add-dest#dest-network-security) para obtener más información.

Si va a crear sus destinos privados con cURL, puede utilizar una línea de mandatos:

```
curl "https://sgmanager.us-south.securegateway.cloud.ibm.com/v1/sgconfig/<gateway_id>/destinations" -H "Authorization: Bearer <security_token>" -H "Content-type: application/json" -d '{"desc":"My Private Destination","ip":"1.1.1.1","port":8000,"private":true}'
```
{: pre}

Una vez creado el destino privado, puede añadir reglas de iptable con mandatos como los siguientes:

```
curl -X PUT "https://sgmanager.us-south.securegateway.cloud.ibm.com/v1/sgconfig/<gateway_id>/destinations/<destination_id>/ipTableRule" -H "Authorization: Bearer <security_token>" -H "Content-type: application/json" -d '{"src":"192.0.0.1","spt":"9000"}' -k
```
{: pre}

y

```
curl -X PUT "https://sgmanager.us-south.securegateway.cloud.ibm.com/v1/sgconfig/<gateway_id>/destinations/<destination_id>/ipTableRule" -H "Authorization: Bearer <security_token>" -H "Content-type: application/json" -d '{"src_range":"192.0.0.1-192.0.0.5","spt":"5000:5005"}' -k
```
{: pre}

Observe que el primer mandato utiliza `src` para proporcionar una sola IP, mientras que el segundo utiliza `src_range` para proporcionar un rango de IP.

## Reglas de iptable para IP dinámicas
{: #iptables-dynamic-ips}

Si la aplicación tiene un conjunto dinámico de IP, pero no las conoce, puede aprovechar la API REST de {{site.data.keyword.SecureGateway}} para actualizar las reglas de iptable sobre la marcha.

Como ejemplo, este breve programa NodeJS actualiza las reglas de iptable para una aplicación Cloud Foundry que ejecuta varias instancias.

```javascript
const request = require('request')

//Esto se configura mejor con variables de entorno.
const GATEWAY_ID = 'XXXXXX'// El ID de la pasarela
const DEST_ID = 'YYYYYY' // El ID de destino al que se restringe el acceso.
const SEC_TOKEN = 'ZZZZZ' // La señal de seguridad de la pasarela.

const APP_ID = JSON.parse(process.env.VCAP_APPLICATION).application_id
const IP_TABLE_BODY = {
  app: APP_ID + ':' + process.env.CF_INSTANCE_INDEX //identifica de forma exclusiva la app y la instancia para la regla de iptable.
  src: process.env.CF_INSTANCE_IP
 }
 
request({
  method: 'PUT',
  uri: `https://sgmanager.us-south.securegateway.cloud.ibm.com/v1/sgconfig/$GATEWAY_ID/destinations/$DEST_ID/ipTableRule`
  headers: {
    'Authorization': `Bearer $SEC_TOKEN`
  }
  json: true, // Content-Type: application/json
  body: IP_TABLE_BODY
  }, console.log.bind(console)) 
```

Se debe ejecutar al iniciar la aplicación; las reglas de IPtables definidas antes se sobrescribirán. Cada regla de iptable identifica de forma exclusiva cada instancia de la aplicación utilizando los valores de `application_id` y de `CF_INSTANCE_INDEX`. La dirección IP externa se recupera de la variable `CF_INSTANCE_IP` y se aplica a la regla de iptable.


{: pre}
