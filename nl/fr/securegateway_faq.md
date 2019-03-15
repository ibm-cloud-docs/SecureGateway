---

copyright:
  years: 2015, 2019
lastupdated: "2019-01-16"

---

# Foire aux questions
{: #sg-faq}

## Couche du modèle OSI
{: #faq-osi}

### Question
{: #osi-question}
Quelle couche du modèle OSI le service Secure Gateway représente-t-il ?

### Réponse
{: #osi-answer}
Le service Secure Gateway représente la couche 4 du modèle OSI.

## Version de TLS prise en charge
{: #faq-tls}

### Question
{: #tls-question}
Quelle version de TLS le service Secure Gateway prend-il en charge ?

### Réponse
{: #tls-answer}
Le service Secure Gateway prend en charge TLS version 1.2.

## Dans quel cas devrai-je désactiver ma passerelle ou ma destination ?
{: #faq-disabled}

### Question
{: #disabled-question}

Pour quelle raison voudriez-vous désactiver une passerelle ou une destination ?

### Réponse
{: #disabled-answer}
Vous pouvez avoir besoin de désactiver une destination ou une passerelle pour l'une des raisons suivantes :

- Vous créez une destination mais vous n'avez pas configuré la sécurité.  Dans ce cas, vous pouvez désactiver la destination jusqu'à ce que la sécurité soit configurée.
- Vous ne voulez pas que le service soit disponible pour les utilisateurs car vous êtes en train d'effectuer des mises à jour dans le service.  Dans ce cas, vous pouvez désactiver temporairement les passerelles nécessaires et attendre que le service soit mis à jour.
- Vous avez configuré toutes vos passerelles et destinations sur le système frontal mais votre système en arrière-plan est toujours en train de compiler.  Dans ce cas, vous désactiverez vos passerelles ou destinations jusqu'à ce que la compilation en arrière-plan soit terminée.

Pour plus d'informations sur la désactivation d'une passerelle ou une destination, voir [gestion de votre instance de service Secure Gateway](/docs/services/SecureGateway/securegateway_managing.html).

## Quelle est l'approche recommandée pour la mise en oeuvre de l'automatisation dans plusieurs espaces ?
{: #faq-automation-spaces}

### Question
{: #automation-spaces-question}
Un environnement client dispose d'une organisation et de trois espaces.  L'un des espaces est destiné au développement, un autre à la préproduction et le dernier à la production.  Le client peut créer une unique instance Secure Gateway ou plusieurs (par exemple, une pour chaque espace) ? Si le client peut créer plusieurs passerelles, est-ce intéressant de réutiliser une application Node.js pour créer une passerelle et une destination dans chaque espace ?

### Réponse
{: #automation-spaces-answer}

- Vous pouvez créer une instance Secure Gateway unique pour chacun des trois espaces.  Toutefois, n'oubliez pas les [limitations applicables à votre plan spécifique](/docs/services/SecureGateway/securegateway_plans.html) de passerelle et de destination.
- Il n'existe pas de considérations supplémentaires liées à la réutilisation d'une application Node.js car Secure Gateway ne requiert aucune liaison de service.


## Quelle est l'approche recommandée pour la mise en oeuvre de l'automatisation dans plusieurs organisations ?
{: #faq-automation-orgs}

### Question
{: #automation-orgs-question}
Un environnement client dispose de trois organisations : une pour le développement, une pour la préproduction et une pour la production.  Une instance de service Secure Gateway est-elle nécessaire pour chaque organisation et la configuration est-elle disponible pour tous les espaces au sein de cette organisation ?

### Réponse
{: #automation-orgs-answer}

- Vous n'avez pas besoin d'avoir une instance de service Secure Gateway dans chaque organisation. Il suffit d'avoir une instance dans une organisation et d'utiliser les passerelles au sein de cette instance à partir de tous vos autres environnements.  Avec cette configuration, vous devez vous souvenir des [limitations applicables à votre plan spécifique](/docs/services/SecureGateway/securegateway_plans.html) de passerelle et de destination.
- Vous pouvez avoir une instance de service Secure Gateway dans chaque organisation et la configuration sera disponible pour tous vos espaces.

## Mon application doit-elle se trouver dans le même espace ?
{: #faq-app-space}

### Question
{: #app-space-question}
Est-il nécessaire d'exécuter l'application Node.js dans le même espace {{site.data.keyword.Bluemix_notm}} que le service Secure Gateway ?

### Réponse
{: #app-space-answer}
Non, vous n'avez pas besoin d'exécuter l'application dans le même espace {{site.data.keyword.Bluemix_notm}} que le service Secure Gateway.

## Puis-je obtenir les journaux du serveur Secure Gateway ?
{: #faq-server-logs}

### Question
{: #server-logs-question}
Est-il possible d'extraire les journaux de niveau d'erreur du serveur Secure Gateway ?

### Réponse
{: #server-logs-answer}
Les journaux de niveau d'erreur sur le serveur ne peuvent pas être extraits. Seules les erreurs qui se produisent au moment de la demande sont visibles.

## Quels sont les états fonctionnels de Secure Gateway ?
{: #faq-states}

### Question
{: #states-question}
Quels sont les différents états du cycle de vie des passerelles et des destinations ?

### Réponse
{: #states-answer}

#### Etat non fonctionnel
{: #states-answer-non-functional}
L'édition 1.7.0 a introduit un nouveau modèle de tarification des plans par niveau. Ce modèle permet de marquer à la fois les passerelles et les destinations comme étant "actives" ou "inactives". Une partie de la nouvelle structure de facturation des plans facture à l'utilisateur le nombre de passerelles et de destinations qu'il possède.

- Les éléments inactifs ne sont pas facturés
- Les destinations inactives n'ont pas de port de cloud mis à disposition.
- Toutes les destinations au sein d'une passerelle inactive sont également inactives et ne peuvent pas être réactivées tant que leur passerelle n'a pas été également réactivée.
- Les éléments inactifs sont considérés comme non fonctionnels. Les éléments inactifs ne peuvent avoir aucun des états fonctionnels.

#### Etats fonctionnels
{: #states-answer-functional}
Une passerelle ou une destination marquée comme étant active sera facturée. Les états actifs des passerelles et des destinations sont les suivants :

- Activée - la passerelle ou la destination est prête à l'emploi dans des conditions normales d'utilisation.
- Désactivée - la passerelle ou la destination a été désactivée.
    - Passerelles - les clients ne pourront pas transmettre de données à la passerelle désactivée.
    - Destinations - les clients ne pourront pas créer de connexions à ces destinations désactivées.

## Comment suis-je informé des activités de données sur le client Secure Gateway ?
{: #faq-data-size}

### Question
{: #data-size-question}
Comment suis-je informé des activités de données via le client Secure Gateway ?

### Réponse
{: #data-size-answer}
Sur le client Secure Gateway, définissez le niveau de journalisation sur TRACE. Suite à l'envoi des demandes, les informations suivantes seront affichées.

Taille des données envoyées depuis l'application de demande :
```
[TRACE] Connection #<ID connexion> transmitted data: <taille> bytes
```

Taille des données envoyées depuis la destination :
```
[TRACE] Connection #<ID connexion> received data: <taille> bytes
```

## Comment puis-je obtenir le nombre de connexions en temps réel sur Secure Gateway ?
{: #faq-connect-num}

### Question
{: #connect-num-question}
Comment puis-je obtenir les informations de connexion, telles que le nombre de connexions en temps réel, la taille des données envoyées et reçues depuis le client Secure Gateway Client ?

### Réponse
{: #connect-num-answer}

- Sur la ligne de commande interactive du client Secure Gateway :
entrez `s` pour imprimer les détails du statu de connexion. 
- Sur l'interface utilisateur du client Secure Gateway, cliquez sur le menu Informations de connexion

Les statistiques de connexion suivantes doivent s'afficher : 
- Taille totale des données transmises.
- Taille totale des données reçues.
- Nombre total de connexions.
- Connexions actives en temps réel.

Remarque : Ces informations de connexion se trouvent au niveau du client, et non de la passerelle. Si vous avez besoin d'informations de connexion au niveau de la passerelle, consultez chaque client connecté à cette passerelle.

## Quelles sont les configurations recommandées pour mieux sécuriser mes connexions ?
{: #faq-secure-app}

### Question
{: #secure-app-question}
Quelles sont les configurations recommandées pour mieux sécuriser mes connexions ?

### Réponse
{: #secure-app-answer}

#### Utiliser l'authentification mutuelle
{: #secure-app-answer-ma}
L'activation de l'authentification mutuelle des deux côtés des destinations sur site renforce la sécurité de Secure Gateway. Du côté de l'authentification de l'utilisateur, activez l'authentification mutuelle afin de restreindre l'accès au noeud de cloud Secure Gateway par une authentification à l'aide d'un certificat client lorsque la demande s'effectue via TLS/HTTPS. Du côté de l'authentification de la ressource, activez l'authentification mutuelle avec entrée des données d'identification appropriées lors de la connexion au noeud final de destination, et vérifiez que l'accès à une ressource sur site est sécurisé/chiffré. Voir [Configuration de l'authentification mutuelle](/docs/services/SecureGateway/securegateway_destination.html#dest-mutual-auth) et [Authentification mutuelle TLS Node.js](/docs/services/SecureGateway/securegateway_tls-ma.html#nodejs-tls-ma) pour plus d'informations.

#### Définir des règles de table d'IP (pour une destination sur site)
{: #secure-app-answer-iptables}
L'hôte et le port de cloud Secure Gateway d'une destination sur site se trouvent dans l'espace public ; par conséquent, l'accès est, par défaut, autorisé à tous.
Pour contrôler le trafic qui accède à Secure Gateway, définissez des règles de table d'IP pour n'autoriser l'accès qu'à une plage spécifique d'adresses IP et de ports afin de sécuriser les ressources sur site. Voir [Règles de table d'IP](/docs/services/SecureGateway/securegateway_destination.html#dest-network-security) pour plus d'informations sur la configuration des règles de table d'IP sur Secure Gateway.

#### Configurer une liste de contrôle d'accès (pour une destination sur site)
{: #secure-app-answer-acl}
Configurez la prise en charge des listes de contrôle d'accès pour autoriser ou refuser l'accès à des ressources sur site de manière à mieux sécuriser les destinations sur site en spécifiant des droits d'accès sur le port et l'hôte d'une destination spécifique. Il est recommandé de définir les routes HTTP/S autorisées ou restreintes sur les entrées de liste de contrôle d'accès, ainsi que d'augmenter la sécurité de la destination sur site. Voir [Liste de contrôle d'accès](/docs/services/SecureGateway/securegateway_acl.html#acl) et [Contrôle des routes et de HTTP/S à l'aide de listes de contrôle d'accès](/docs/services/SecureGateway/securegateway_acl.html#acl-route-control) pour plus d'informations.

#### Définir un mot de passe sur l'interface utilisateur du client Secure Gateway
{: #secure-app-answer-ui-pw}
Il est recommandé de définir un mot de passe pour l'interface utilisateur afin de restreindre l'accès à l'interface utilisateur du client Secure Gateway. Voir [Interaction avec le client](/docs/services/SecureGateway/securegateway_interaction.html#client-interacting) pour plus d'informations sur la définition du mot de passe à l'aide d'une configuration de démarrage ou des commandes interactives sur la ligne de commande du terminal client Secure Gateway.

## Qu'est-ce que la migration de passerelle ? Pourquoi le domaine a-t-il été changé après décembre 2018 ?
{: #faq-gateway-migration}

### Question
{: #gateway-migration-question}
Un bouton de migration a été ajouté sur la panneau de la passerelle depuis décembre 2018. A quoi celui-ci sert-il ? Pourquoi le domaine a-t-il changé ?

### Réponse
{: #gateway-migration-answer}

Après la maintenance de décembre 2018, l'hôte sur le cloud de Secure Gateway a été renommé pour utiliser `securegateway.appdomain.cloud` à la place de `integration.ibmcloud.com`, et pour utiliser `securegateway.cloud.ibm.com` pour l'authentification de passerelle à la place de `bluemix.net`. Afin d'assurer la compatibilité avec les versions antérieures, la passerelle existante continuera d'utiliser l'ancien domaine jusqu'à la migration de la passerelle, et le client SG v180fp9 et versions antérieures continuera à utiliser `bluemix.net` pour l'authentification de la passerelle.

Une fois la migration effectuée, l'hôte sur le cloud des destinations sur site sera modifié afin d'utiliser le nouveau domaine et les utilisateurs/applications devront être mis à jour pour envoyer la demande vers le nouvel hôte sur le cloud.

Actuellement, la migration n'est pas obligatoire et la date exacte à laquelle l'ancien hôte ne sera plus pris en charge n'est pas encore connue. Les clients qui utiliseront encore l'ancien domaine seront informés dès que la date aura été fixée.

## Où puis-je recevoir des notifications ?
{: #faq-notification}

### Question
{: #notification-question}
Où puis-je voir les notifications Secure Gateway, en particulier celles concernant les événements de maintenance avec interruption ?

### Réponse
{: #notification-answer}

Vous pouvez recevoir les notifications sur notre [page de statut](https://console.bluemix.net/status). Consultez la rubrique `Secure Gateway` sur cette page.

Lorsque le client Secure Gateway a été déconnecté de manière inattendue, accédez à la page de statut pour vérifier si une maintenance avec interruption était planifiée à ce moment-là.
