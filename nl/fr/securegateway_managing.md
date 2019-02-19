---

copyright:
  years: 2015, 2017
lastupdated: "2017-04-10"

---

# Gestion de votre service {{site.data.keyword.SecureGateway}}
{: #manage-sg-service}

## {{site.data.keyword.SecureGateway}} Tableau de bord
A partir de votre tableau de bord {{site.data.keyword.SecureGateway}}, vous pouvez rapidement visualiser les informations suivantes :

- Le nombre en cours de connexions à vos destinations sur toutes vos passerelles.
- Le flux de données entrantes sur toutes vos passerelles au cours des 12 dernières heures.
- Le flux de données sortantes sur toutes vos passerelles au cours des 12 dernières heures.
- Un graphique illustrant l'utilisation de chaque passerelle au cours des 12 dernières heures.
- Vos passerelles existantes, leur état en cours et le nombre de destinations qu'elles contiennent.

![{{site.data.keyword.SecureGateway}} Tableau de bord avec utilisation](./images/dashboardUsage.png?raw=true "{{site.data.keyword.SecureGateway}} Tableau de bord avec utilisation")

### Profil de service
Si vous cliquez sur le bouton Profil, les informations suivantes s'affichent :

Zone | Description
-- | --
Niveau du plan | Plan de service actuellement sélectionné.
Limite passerelle | Nombre de passerelles accordées avant facturation pour passerelles supplémentaires.
Limite client | Nombre de clients que vous pouvez connecter à une même passerelle.
Limite destination | Nombre de destinations que peut contenir une unique passerelle.
Limite de données | Quantité de transfert de données (en Go) que votre plan prend en charge avant facturation pour dépassement de quantité de données.
Excédent passerelle | Nombre de dépassements de passerelle qui vous est actuellement facturé.
Excédent données | Nombre de dépassements de données imputés pour le cycle de facturation du mois en cours.

### Panneau d'informations sur la passerelle
Si vous cliquez sur le bouton Paramètres de n'importe quelle vignette de passerelle, les informations suivantes s'affichent :

- Le jeton de sécurité requis pour utiliser l'API ou connecter un client, selon que le jeton est imposé aux connexions client.
- L'ID de passerelle requis pour utiliser l'API ou connecter un client.
- Le noeud sur lequel votre passerelle a été créée.  Il s'agit du serveur auquel votre client se connectera.
- Quand la passerelle a été créée et quel utilisateur l'a créée.
- Quand la passerelle a été modifiée pour la dernière fois et quel utilisateur l'a modifiée.
- Un lien permettant de régénérer le certificat et la clé associés à la passerelle.  Ces fichiers sont utilisés uniquement avec des destinations existantes qui ne contiennent pas leur propre paire certificat-clé pour l'authentification mutuelle sur le client.

Les tâches suivantes peuvent également être effectuées à partir du panneau d'informations sur la passerelle :

- Editer ou afficher davantage d'informations sur votre passerelle et sur le jeton de sécurité.  Un nouveau panneau s'ouvre lorsque vous sélectionnez cette option.
- Désactiver ou activer la passerelle.
- Supprimer la passerelle.

## Tableau de bord d'une passerelle particulière
Si vous cliquez sur l'une des vignettes de passerelle, le tableau de bord de cette passerelle s'affiche.  Comme depuis le tableau de bord {{site.data.keyword.SecureGateway}} principal, vous pouvez rapidement visualiser les informations suivantes :

- Le nombre en cours de connexions aux destinations sur cette passerelle.
- Le flux de données entrantes sur cette passerelle au cours des 12 dernières heures.
- Le flux de données sortantes sur cette passerelle au cours des 12 dernières heures.
- Un graphique illustrant l'utilisation de chaque destination au cours des 12 dernières heures.
- Vos destinations existantes, leur état en cours et leur nombre en cours de connexions actives.

![Tableau de bord d'une passerelle particulière](./images/viewGateway.png?raw=true "Tableau de bord d'une passerelle particulière")

### Panneau d'informations sur la destination
Si vous cliquez sur le bouton Paramètres de n'importe quelle vignette de destination, les informations suivantes s'affichent :

- L'ID de destination requis pour utiliser l'API.
- Les destinations sur site auront un numéro de port et un nom d'hôte de cloud. Votre application a besoin de cette information pour se connecter à votre ressource sur site.
- Les destinations sur le cloud auront un port client.  Il s'agit du port sur lequel le client {{site.data.keyword.SecureGateway}} sera en mode écoute pour connecter à votre application sur site à votre ressource de cloud.
- L'hôte et le port de ressource vers lesquels pointe cette destination.
- Quand la destination a été créée et quel utilisateur l'a créée.
- Quand la destination a été modifiée pour la dernière fois et quel utilisateur l'a modifiée.

Les tâches suivantes peuvent également être effectuées à partir du panneau d'informations sur la destination :

- Editer ou afficher davantage d'informations sur la destination.  Un nouveau panneau s'ouvre lorsque vous sélectionnez cette option.
- Désactiver ou activer la destination.
- Supprimer la destination.
