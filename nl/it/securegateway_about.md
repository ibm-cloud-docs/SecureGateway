---

copyright:
  years: 2015, 2017
lastupdated: "2017-04-10"

---

# Informazioni su {{site.data.keyword.SecureGateway}}
{: #about-sg}

## In che modo è sicuro?
{: #about-secure}
{{site.data.keyword.SecureGatewayfull}} mantiene una singola connessione crittografata (TLS v1.2) persistente tra il client {{site.data.keyword.SecureGateway}} (nella rete in loco) e i server {{site.data.keyword.SecureGateway}}.  Con questa connessione bidirezionale, possiamo trasmettere in modo protetto i dati tra le tue risorse cloud e le tue risorse in loco.  Per le connessioni su entrambe le estremità (tra la risorsa cloud e i server SG e tra le risorse in loco e il client SG), l'utente definisce il protocollo (TCP, TLS, HTTP, HTTPS, UDP) e le misure di sicurezza (autenticazione reciproca, iptable) su di esse implementati.  

## Supporto bidirezionale
{: #about-bidirectional}
Combinando la tradizionale destinazione in loco con la nuova destinazione cloud, otteniamo un supporto bidirezionale completo.  La nuova destinazione cloud consente a un servizio e/o un'applicazione in loco di inviare informazioni/richieste ad un'applicazione cloud tramite il client {{site.data.keyword.SecureGateway}}.  Quando si crea una nuova destinazione cloud, la destinazione conterrà `resource host` (il nome host o l'IP dell'applicazione cloud), `resource port` (la porta su cui sarà in ascolto l'applicazione cloud) e `client port` (la porta su cui sarà in ascolto il client {{site.data.keyword.SecureGateway}}).  Dopo che questa destinazione è stata stabilita, il servizio in loco può stabilire una
connessione alla porta client e iniziare a inviare dati; tale invio verrà eseguito tramite il client, al server {{site.data.keyword.SecureGateway}},
e quindi all'applicazione cloud specificata.

La porta client fornita per una destinazione deve essere univoca nel gateway associato.  Ad esempio, un singolo gateway non può avere due destinazioni inverse in ascolto sulla porta 12000,
ma due gateway possono ciascuno avere una propria destinazione in ascolto sulla porta 12000.  Tuttavia, in questo ultimo caso, se entrambi i gateway sono connessi allo stesso client, solo una
delle destinazioni sarà in grado di essere in ascolto sulla porta 12000.

### Destinazione cloud
{: #about-cloud-dest}
![Destinazione cloud](./images/reverseDestination.png?raw=true "Destinazione cloud")

### Destinazione in loco
{: #about-on-prem-dest}
![Destinazione in loco](./images/onPremDestination.png?raw=true "Destinazione in loco")

## Alta disponibilità
{: #about-ha}
Per ottenere l'alta disponibilità, crea più connessioni client allo stesso ID gateway.  Tale operazione può essere eseguita connettendo molteplici client a singolo gateway allo stesso ID gateway e/o immettendo più connessioni su un singolo client a più gateway allo stesso ID gateway.  Le connessioni saranno distribuite tra tutti i client connessi in modalità round-robin.
