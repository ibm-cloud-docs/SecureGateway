---

copyright:
  years: 2015, 2018
lastupdated: "2018-08-10"

---

# Plans de service {{site.data.keyword.SecureGateway}}
{: #secure-gateway-service-plans}

Avec la version 1.7.0, un nouveau modèle de tarification des plans par niveau, qui permet d'adapter {{site.data.keyword.SecureGateway}} aux besoin de votre entreprise, a été introduit.  Le tableau ci-dessous indique les limitations associées à chacun des plans disponibles publiquement :

![Modèle de plan par niveau](./images/planDetails.png?raw=true "Modèle de plan par niveau")

## Changement de plan
Lorsque vous changez de plan, le nouveau plan sera considéré soit comme une mise à niveau soit comme un rétromigration.  Le statut de la modification est déterminé par le nombre de passerelles par défaut autorisé par chaque plan (par exemple, le passage du plan Professional (5) au plan Enterprise (25) est une mise à niveau).  Une mise à niveau n'entraîne aucune interruption du service tandis qu'une rétromigration met toutes les passerelles à l'état [inactif](/docs/services/SecureGateway/securegateway_faq.html#states), ce qui vous oblige à réactiver vos passerelles et destinations (jusqu'à la limite de votre nouveau plan) avant restauration du service.

<b>Remarque</b> : toute transition du plan Standard vers l'un des autres nouveaux plans est considéré comme une rétromigration.


## Notification de dépassement de limite
Lorsque vous avez créé le nombre maximum de passerelles/destinations autorisé, un message d'avertissement s'affiche dans le tableau de bord de la passerelle/destination.

Lorsque le nombre maximum de clients est atteint et qu'un nouveau client tente de se connecter à la passerelle, un message d'erreur est inscrit dans le journal du client et le client est rejeté par la passerelle.

Lorsque l'utilisation des données excède la quantité autorisée de transfert de données, un message d'erreur est inscrit dans le journal du client et le client est rejeté par la passerelle.
