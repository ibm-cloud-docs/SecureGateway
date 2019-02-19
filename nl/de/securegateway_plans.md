---

copyright:
  years: 2015, 2018
lastupdated: "2018-08-10"

---

# {{site.data.keyword.SecureGateway}}-Servicepläne
{: #secure-gateway-service-plans}

Mit Release 1.7.0 wurde ein neues Preismodell mit einem Stufenplan eingeführt, das eine Skalierung von {{site.data.keyword.SecureGateway}} abhängig von den Anforderungen des jeweiligen Unternehmens ermöglicht. In der folgenden Tabelle werden die Einschränkungen aufgeführt, die für jeden der öffentlich verfügbaren Plänen gelten:

![Stufenplanmodell](./images/planDetails.png?raw=true "Stufenplanmodell")

## Pläne ändern
Wenn Sie Pläne ändern, wird der neue Plan entweder als Upgrade oder Downgrade betrachtet. Dies ergibt sich aus der Standardanzahl der Gateways, die für jeden Plan zulässig sind (ein Wechsel von Professional (5) zu Enterprise (25) ist zum Beispiel ein Upgrade). Bei einem Upgrade kommt es nicht zu einer Serviceunterbrechung; bei einem Downgrade werden dagegen alle Gateways auf [inaktiv](/docs/services/SecureGateway/securegateway_faq.html#states) aktualisiert, worauf Sie die Gateways und Ziele reaktivieren müssen (bis zur Obergrenze des neuen Plans), damit der Service wiederhergestellt wird.

<b>Anmerkung:</b> Ein Wechseln vom Standardplan zu einem neuen Plan wird als Downgrade betrachtet.


## Benachrichtigung bei überschrittenem Grenzwert
Wenn Sie die maximal zulässige Menge an Gateways bzw. Zielen erstellen, wird im Dashboard des Gateways bzw. Ziels ein Warnungsprotokoll angezeigt.

Wenn der Clientumfang erreicht ist und von einem neuen Client versucht wird, eine Verbindung zum Gateway herzustellen, wird im Clientprotokoll ein Fehlerprotokoll angezeigt, und der neue Client wird vom Gateway beendet.

Wenn die Datennutzung die vereinbarte Datenübertragungsleistung überschreitet, wird im Clientprotokoll ein Fehlerprotokoll angezeigt, und der Client wird vom Gateway beendet.
