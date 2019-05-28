---

copyright:
  years: 2015, 2018
lastupdated: "2018-08-10"

---

# Piani di servizio {{site.data.keyword.SecureGateway}}
{: #secure-gateway-service-plans}

Con la release della v1.7.0, è stato introdotto un nuovo modello di prezzo dei piani a livelli, che consente il ridimensionamento di {{site.data.keyword.SecureGateway}} in base alle esigenze di qualsiasi azienda.  La tabella di seguito fornisce le limitazioni associate a ciascuno dei piani pubblicamente disponibili:

![Modello di piani a livelli](./images/planDetails.png?raw=true "Modello di piani a livelli")

## Modifica dei piani
{: #changing-plans}
Quando modifichi i piani, il tuo nuovo piano verrà considerato essere un upgrade o un downgrade.  Ciò è determinato dal numero predefinito di gateway consentito da ciascun piano (ad es. da Professional (5) e Enterprise (25) è un upgrade).  Quando esegui un upgrade, non ci sarà alcuna interruzione del servizio; tuttavia, un downgrade aggiornerà tutti i gateway perché siano [inattivi](/docs/services/SecureGateway?topic=securegateway-sg-faq#faq-states), il che richiederà che riattivi i tuoi gateway e la destinazione (fino al tuo nuovo limite del piano) prima che il servizio venga ripristinato.

<b>Nota</b>: quando esegui la transizione dal piano Standard a uno qualsiasi dei nuovi piani, tale passaggio verrà considerato un downgrade.


## Notifica per un limite superato
{: #limit-exceeded-notifications}
Quando crei la quantità massima consentita di gateway/destinazione, nel relativo dashboard sarà visualizzato un log del livello di avvertenza.

Quando viene raggiunta la quantità di client e c'è un nuovo tentativo del client di connettersi al gateway, ci sarà un log del livello di errore visualizzato nel log del client e il nuovo client verrà chiuso dal gateway.

Quando l'utilizzo dei dati supera la quota di trasferimento dati consentita, ci sarà un log del livello di errore visualizzato nel log del client e il client verrà chiuso dal gateway.
