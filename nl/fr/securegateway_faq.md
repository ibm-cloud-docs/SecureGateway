---

copyright:
  years: 2015, 2018
lastupdated: "2018-11-08"

---

# Foire aux questions
{: #sg-faq}

## Couche du modèle OSI
{: #osi}

### Question
Quelle couche du modèle OSI le service Secure Gateway représente-t-il ?

### Réponse
Le service Secure Gateway représente la couche 4 du modèle OSI.

## Version de TLS prise en charge
{: #tls}

### Question
Quelle version de TLS le service Secure Gateway prend-il en charge ?

### Réponse
Le service Secure Gateway prend en charge TLS version 1.2.

## Dans quel cas devrai-je désactiver ma passerelle ou ma destination ?
{: #disabled}

### Question

Pour quelle raison voudriez-vous désactiver une passerelle ou une destination ?

### Réponse
Vous pouvez avoir besoin de désactiver une destination ou une passerelle pour l'une des raisons suivantes :

- Vous créez une destination mais vous n'avez pas configuré la sécurité.  Dans ce cas, vous pouvez désactiver la destination jusqu'à ce que la sécurité soit configurée.
- Vous ne voulez pas que le service soit disponible pour les utilisateurs car vous êtes en train d'effectuer des mises à jour dans le service.  Dans ce cas, vous pouvez désactiver temporairement les passerelles nécessaires et attendre que le service soit mis à jour.
- Vous avez configuré toutes vos passerelles et destinations sur le système frontal mais votre système en arrière-plan est toujours en train de compiler.  Dans ce cas, vous désactiverez vos passerelles ou destinations jusqu'à ce que la compilation en arrière-plan soit terminée.

Pour plus d'informations sur la désactivation d'une passerelle ou une destination, voir [gestion de votre instance de service Secure Gateway](/docs/services/SecureGateway/securegateway_managing.html).

## Quelle est l'approche recommandée pour la mise en oeuvre de l'automatisation dans plusieurs espaces ?
{: #automation-spaces}

### Question
Un environnement client dispose d'une organisation et de trois espaces.  L'un des espaces est destiné au développement, un autre à la préproduction et le dernier à la production.  Le client peut créer une unique instance Secure Gateway ou plusieurs (par exemple, une pour chaque espace).  Si le client peut créer plusieurs passerelles, est-ce intéressant de réutiliser une application Node.js pour créer une passerelle et une destination dans chaque espace ?

### Réponse

- Vous pouvez créer une instance Secure Gateway unique pour chacun des trois espaces.  Toutefois, n'oubliez pas les [limitations applicables à votre plan spécifique](/docs/services/SecureGateway/securegateway_plans.html) de passerelle et de destination.
- Il n'existe pas de considérations supplémentaires liées à la réutilisation d'une application Node.js car Secure Gateway ne requiert aucune liaison de service.


## Quelle est l'approche recommandée pour la mise en oeuvre de l'automatisation dans plusieurs organisations ?
{: #automation-orgs}

### Question
Un environnement client dispose de trois organisations : une pour le développement, une pour la préproduction et une pour la production.  Une instance de service Secure Gateway est-elle nécessaire pour chaque organisation et la configuration est-elle disponible pour tous les espaces au sein de cette organisation ?

### Réponse

- Vous n'avez pas besoin d'avoir une instance de service Secure Gateway dans chaque organisation. Il suffit d'avoir une instance dans une organisation et d'utiliser les passerelles au sein de cette instance à partir de tous vos autres environnements.  Avec cette configuration, vous devez vous souvenir des [limitations applicables à votre plan spécifique](/docs/services/SecureGateway/securegateway_plans.html) de passerelle et de destination.
- Vous pouvez avoir une instance de service Secure Gateway dans chaque organisation et la configuration sera disponible pour tous vos espaces.

## Mon application doit-elle se trouver dans le même espace ?
{: #app-space}

### Question
Est-il nécessaire d'exécuter l'application Node.js dans le même espace {{site.data.keyword.Bluemix_notm}} que le service Secure Gateway ?

### Réponse
Non, vous n'avez pas besoin d'exécuter l'application dans le même espace {{site.data.keyword.Bluemix_notm}} que le service Secure Gateway.

## Puis-je obtenir les journaux du serveur Secure Gateway ?
{: #server-logs}

### Question
Est-il possible d'extraire les journaux d'erreur du serveur Secure Gateway ?

### Réponse
Les journaux d'erreur du serveur ne peuvent être extraits.  Seules les erreurs qui se produisent au moment de la demande sont visibles.

## Quels sont les états fonctionnels de Secure Gateway ?
{: #states}

### Question
Quels sont les différents états du cycle de vie des passerelles et des destinations ?

### Réponse

#### Etat non fonctionnel
L'édition 1.7.0 a introduit un nouveau modèle de tarification des plans par niveau. Ce modèle permet de marquer à la fois les passerelles et les destinations comme étant "actives" ou "inactives". Une partie de la nouvelle structure de facturation des plans facture à l'utilisateur le nombre de passerelles et de destinations qu'il possède.

- Les éléments inactifs ne sont pas facturés
- Les destinations inactives n'ont pas de port de cloud mis à disposition.
- Toutes les destinations au sein d'une passerelle inactive sont également inactives et ne peuvent pas être réactivées tant que leur passerelle n'a pas été également réactivée.
- Les éléments inactifs sont considérés comme non fonctionnels. Les éléments inactifs ne peuvent avoir aucun des états fonctionnels.

#### Etats fonctionnels
Une passerelle ou une destination marquée comme étant active sera facturée. Les états actifs des passerelles et des destinations sont les suivants :

- Activée - la passerelle ou la destination est prête à l'emploi dans des conditions normales d'utilisation.
- Désactivée - la passerelle ou la destination a été désactivée.
    - Passerelles - les clients ne pourront pas transmettre de données à la passerelle désactivée.
    - Destinations - les clients ne pourront pas créer de connexions à ces destinations désactivées.

## Comment suis-je informé des activités de données sur le client Secure Gateway ?
{: #data-size}

### Question
Comment suis-je informé des activités de données via le client Secure Gateway ?

### Réponse
Sur le client Secure Gateway, définissez le niveau de journalisation sur TRACE. Suite à l'envoi des demandes, les informations suivantes seront affichées.

Taille des données envoyées depuis l'application de demande :
```
[TRACE] Connection #<ID connexion> transmitted data: <taille> bytes
```

Taille des données envoyées depuis la destination :
```
[TRACE] Connection #<ID connexion> received data: <taille> bytes
```

## Est-ce que j'obtiens le nombre de connexions en temps réel sur Secure Gateway ?
{: #connect-num}

### Question
Comment puis-je obtenir les informations de connexion en temps réel, ainsi que la taille des données envoyées et reçues depuis le client Secure Gateway ?

### Réponse

- Sur la ligne de commande interactive du client Secure Gateway :
entrez `s` pour imprimer les détails du statu de connexion. 
- Sur l'interface utilisateur du client Secure Gateway, cliquez sur le menu Informations de connexion

Les statistiques de connexion suivantes doivent s'afficher : 
- Taille totale des données transmises.
- Taille totale des données reçues.
- Nombre total de connexions.
- Connexions actives en temps réel.

Remarque : Le nombre de connexions en cours sur l'interface utilisateur Secure Gateway n'est pas fourni en temps réel. Pour obtenir les informations concernant les connexions en temps réel, utilisez les méthodes ci-dessus sur le client Secure Gateway.

## Quelles sont les configurations recommandées pour mieux sécuriser mes connexions ?
{: #secure-app}

### Question
Quelles sont les configurations recommandées pour mieux sécuriser mes connexions ?

### Réponse

#### Utiliser l'authentification mutuelle
L'activation de l'authentification mutuelle des deux côtés des destinations sur site renforce la sécurité de Secure Gateway. Du côté de l'authentification de l'utilisateur, activez l'authentification mutuelle afin de restreindre l'accès au noeud de cloud Secure Gateway par une authentification à l'aide d'un certificat client lorsque la demande s'effectue via TLS/HTTPS. Du côté de l'authentification de la ressource, activez l'authentification mutuelle avec entrée des données d'identification appropriées lors de la connexion au noeud final de destination, de manière à garantir un accès chiffré/sécurisé à une ressource sur site. Voir [Configuration de l'authentification mutuelle](/docs/services/SecureGateway/securegateway_destination.html#mutual-auth) et [Authentification mutuelle TLS Node.js](/docs/services/SecureGateway/securegateway_tls-ma.html#node-js-tls-mutual-authentication) pour plus d'informations.

#### Définir des règles de table d'IP (pour une destination sur site)
L'hôte et le port de cloud Secure Gateway d'une destination sur site se trouvent dans l'espace public ; par conséquent, l'accès est, par défaut, autorisé à tous.
Pour contrôler le trafic qui accède à Secure Gateway, définissez des règles de table d'IP pour n'autoriser l'accès qu'à une plage spécifique de ports et d'adresses IP afin de sécuriser des ressources sur site. Voir [Règles de table d'IP](/docs/services/SecureGateway/securegateway_destination.html#configuring-network-security) pour plus d'informations sur la configuration de règles de table d'IP sur Secure Gateway.

#### Configurer une liste de contrôle d'accès (pour une destination sur site)
Configurez la prise en charge des listes de contrôle d'accès pour autoriser ou refuser l'accès à des ressources sur site de manière à mieux sécuriser les destinations sur site en spécifiant des droits d'accès sur le port et l'hôte d'une destination spécifique. Il est recommandé de définir les routes HTTP/S autorisées ou d'en interdire sur les entrées de liste de contrôle d'accès afin d'augmenter la sécurité de la destination sur site. Voir [Liste de contrôle d'accès](/docs/services/SecureGateway/securegateway_acl.html#access-control-list) et [Contrôle des routes et de HTTPS à l'aide de listes de contrôle d'accès](/docs/services/SecureGateway/securegateway_acl.html#routes) pour plus d'informations.

#### Définir un mot de passe sur l'interface utilisateur du client Secure Gateway
Il est recommandé de définir un mot de passe pour l'interface utilisateur afin de restreindre l'accès à l'interface utilisateur du client Secure Gateway. Voir [Interaction avec le client](/docs/services/SecureGateway/securegateway_interaction.html#interacting-with-the-client) pour plus d'informations sur la définition du mot de passe à l'aide d'une configuration de démarrage ou des commandes interactives sur la ligne de commande du terminal client Secure Gateway.
