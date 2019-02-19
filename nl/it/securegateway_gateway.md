---

copyright:
  years: 2015, 2017
lastupdated: "2017-04-10"

---

# Aggiunta di un gateway

Puoi pensare a un gateway come a un modo per identificare una rete o un ambiente specifici. Si tratta di ciò che un client Secure Gateway utilizzerà per stabilire la connettività con i server Secure Gateway e può contenere più definizioni di risorse o destinazioni.

![Dashboard Secure Gateway](./images/newDashboard.png?raw=true "Dashboard Secure Gateway")

Dal dashboard Secure Gateway, fai clic sul pulsante Add Gateway per aprire il pannello Add Gateway.

![Add Gateway](./images/addGateway.png?raw=true "Add Gateway")

L'unico input richiesto su questo pannello è il tuo nome gateway. Per impostazione predefinita, sono selezionate sia `Require security token` che `Token Expiration`.

Richiedendo il token di sicurezza per connettere i client, ogni volta che avvii un client Secure Gateway dovrai fornire sia un ID gateway che il token di sicurezza. Se deselezioni la casella `Require security token`, dovrai fornire solo l'ID gateway per la connessione del client.

La data di scadenza predefinita per il token di sicurezza è di 90 giorni da quando viene creato. Per modificare la data di scadenza, tieni selezionata la casella `Token Expiration` e modifica il campo di testo con il numero di giorni dopo il quale si desidera che il token scada (minimo 1, massimo 365).  Per creare un token che non scade, deseleziona la casella `Token Expiration`.  

Per terminare la creazione del tuo gateway, fai clic su Add Gateway. Una volta creato il gateway, la pagina passerà automaticamente al nuovo gateway.

![New Gateway](./images/newGateway.png?raw=true "New Gateway")

Ora che hai il tuo gateway appena creato, puoi [connettere il tuo primo client](/docs/services/SecureGateway/securegateway_client.html).
