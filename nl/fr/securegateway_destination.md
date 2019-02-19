---

copyright:
  years: 2015, 2017
lastupdated: "2017-04-10"

---

# Ajout d'une destination

Une destination définit comment se connecter à une ressource sur site ou sur le cloud spécifique. Une fois la destination créée, les serveurs {{site.data.keyword.SecureGateway}} lui fournissent un noeud final public unique où elle sera en mode écoute pour les connexions pendant que la passerelle est connectée.

![Tableau de bord sans destinations](./images/emptyDestinations.png?raw=true "Tableau de bord sans destinations")

Dans votre nouvelle passerelle, sur l'onglet Destinations, cliquez sur le bouton Ajouter une destination pour ouvrir le panneau correspondant.  Deux méthodes permettent de créer une destination : une configuration guidée qui montre comment chaque éléments s'insère dans l'architecture globale et une configuration avancée qui propose toutes les zones et options sur un même panneau.  

La configuration guidée ne permet <b>pas</b> de configurer des informations de proxy, des indicateurs de nom de serveur ou le téléchargement d'une paire certificat-clé propre à une destination.  Après la création, toutes les zones sont disponibles via le panneau Editer la destination.

## Panneau de configuration guidée

![Configuration guidée](./images/guidedLanding.png?raw=true "Panneau d'accueil de configuration guidée")

## Panneau de configuration avancée

![Configuration avancée](./images/advancedLanding.png?raw=true "Panneau d'accueil de configuration avancée")

## Destinations sur site ou sur le cloud
{: #dest-types}

La première question lors de la création de votre destination est de vous demander où résident les ressources auxquelles vous avez besoin de vous connecter.

### Destination sur site
Une destination sur site s'utilise lorsqu'une application dans l'espace public a besoin d'accéder à une ressource restreinte qui se trouve sur site.
![On Destination sur site](./images/onPremDestination.png?raw=true "Destination sur site")

### Destination sur le cloud
Une destination sur le cloud s'utilise lorsqu'une application dans un réseau restreint a besoin d'accéder à une ressource disponible dans un espace public.
![Destination inverse](./images/reverseDestination.png?raw=true "Destination sur le cloud")

## Définition de votre destination
Les informations suivantes sont requises pour les deux types de destination :

- <b>Nom d'hôte de la ressource</b> : adresse IP ou nom d'hôte de la ressource à laquelle vous voulez vous connecter.
- <b>Port de la ressource</b> : port sur lequel votre ressource est en mode écoute.
- <b>Protocole</b> : type de connexion qu'établira votre application.  Pour les diverses [options de protocole](#protocols), voir le tableau ci-dessous.  Pour configurer le type de connexion attendu par votre ressource, cochez la section [Authentification des ressources](#resource-auth).

Si vous avez sélectionné une destination sur le cloud, vous devez également indiquer un <b>Port client</b>.  Il s'agit du port sur lequel le client {{site.data.keyword.SecureGateway}} sera en mode écoute afin d'autoriser les connexions au port et au nom d'hôte de ressource associés.

## Options de protocole
{: #protocols}

Ce tableau répertorie toutes les options disponibles concernant la manière dont votre application peut initier des connexions/demandes avec {{site.data.keyword.SecureGateway}}.

Protocole | Description
-- | --
TCP | Pas d'authentification. Votre application peut communiquer directement avec les serveurs {{site.data.keyword.SecureGateway}} sans aucun certificat.
TLS : côté serveur | La connexion TLS (Transport Layer Security) est activée et le serveur fournit un certificat pour prouver ses droits.
TLS : authentification mutuelle | Le serveur fournit un ensemble de certificats et votre application doit également fournir un certificat dans le cadre de sa connexion.
HTTP | Connexion TCP où l'en-tête d'hôte est réécrit pour correspondre au nom d'hôte sur site.
HTTPS | Connexion TLS : connexion côté serveur où l'en-tête d'hôte est réécrit pour correspondre au nom d'hôte sur site.
HTTPS : authentification mutuelle | Connexion TLS : authentification mutuelle où l'en-tête d'hôte est réécrit pour correspondre au nom d'hôte sur site.


## Configuration de l'authentification mutuelle
{: #mutual-auth}

Pour les protocoles qui imposent une authentification mutuelle, vous devez télécharger votre propre certificat sinon le serveur crée automatiquement une paire certificat autosigné-clé à l'usage de votre application.  Vous pouvez télécharger cette paire en même temps que le certificat du serveur.
![Panneaux d'authentification mutuelle](./images/mutualAuth.png?raw=true "Panneaux d'authentification mutuelle")

### Authentification de l'utilisateur
{: #user-auth}

La section d'authentification de l'utilisateur permet de gérer l'autorisation de votre application lorsqu'elle émet une demande/se connecte à {{site.data.keyword.SecureGateway}}.  Cette zone accepte un unique certificat qui doit être celui que votre application présentera pour toute connexion/demande.

### Authentification des ressources
{: #resource-auth}

L'authentification des ressources détermine comment {{site.data.keyword.SecureGateway}} tente de se connecter à la ressource définie.  Trois options sont disponibles :  Aucune, TLS (côté serveur) et authentification mutuelle. En fonction de votre choix, différentes options d'authentification sont proposées.

L'activation de TLS sur la connexion à votre ressource est distincte de TLS utilisé pour l'authentification des utilisateurs.  TLS pour l'authentification des utilisateurs sécurise l'accès entre votre application demandeuse initiale et {{site.data.keyword.SecureGateway}} (par exemple, entre votre application {{site.data.keyword.Bluemix_notm}} et les serveurs {{site.data.keyword.SecureGateway}}) alors que TLS pour l'authentification des ressources sécurise la connexion entre {{site.data.keyword.SecureGateway}} et votre ressource définie (par exemple, entre le client {{site.data.keyword.SecureGateway}} et votre base de données sur site).

#### Authentification sur le cloud/sur site

Cette option est disponible lorsque vous sélectionnez TLS ou l'authentification mutuelle pour l'authentification des ressources.  Le nom de la zone correspondra au [type de destination](#dest-types) que vous avez choisi.  Cette zone vous permet de télécharger jusqu'à 6 certificats en vue de valider le certificat de la ressource à laquelle vous vous connectez.  Ces fichiers seront ajoutés à l'autorité de certification de la connexion à la ressource et doivent contenir le certificat ou la chaîne de certificats que votre ressource présentera.

#### Indicateur de noms de serveur
Cette option est disponible lorsque vous sélectionnez TLS ou l'authentification mutuelle pour l'authentification des ressources.  Elle est utilisée pour permettre de fournir un nom d'hôte distinct à l'établissement de liaison TLS de la connexion de ressource.

### Clé et certificat client
L'endroit où s'affichent les zones de clé et de certificat client dépend du [type de destination](#dest-types) que vous avez choisi.  Dans les deux cas, les fichiers fournis ici seront utilisés par le client SG pour s'identifier pour des connexions TLS.  Si aucun fichier n'est téléchargé, les serveurs {{site.data.keyword.SecureGateway}} génèrent automatiquement une paire autosignée avec un nom usuel de `système hôte local`.  Pour les instructions relatives à la génération d'une paire certificat-clé, [cliquez ici](/docs/services/SecureGateway/securegateway_keygen.html).

Pour une destination sur site, l'option s'affiche sous Authentification des ressources si vous avez sélectionné l'authentification mutuelle pour l'authentification des ressources.  Dans ce cas, le client n'utilisera pas cette paire certificat-clé pour ses connexions sortantes vers la ressource définie.  L'autorité de certification de cette connexion contiendra le certificat fourni dans la zone [Authentification cloud/sur site](#resource-auth).

Pour une destination sur le cloud, l'option s'affiche sous Authentification des utilisateurs si vous avez sélectionné un protocole TLS.  Dans ce cas, le client utilisera cette paire certificat-clé pour établir des programmes d'écoute TLS avec le fichier téléchargé de l'[Authentification des utilisateurs](#user-auth) dans l'autorité de certification.  

## Configuration de la sécurité du réseau
Pour que seules certaines adresses IP puissent se connectent à vos port et hôte de cloud, vous pouvez choisir d'imposer des règles de table d'IP (iptable) à votre destination sur site.
![Panneau Sécurité du réseau](./images/networkSecurity.png?raw=true "Panneau Sécurité du réseau")

Pour imposer des règles de table d'IP, cochez la case <b>Restreindre l'accès cloud à cette destination avec des règles iptable</b> du panneau Sécurité du réseau.  Une fois la case cochée, vous pouvez commencer à ajouter des adresses IP qui seront autorisées à se connecter.  Si aucune adresse IP n'est fournie, toutes les connexions à ces port et hôte de cloud seront rejetées tant que la case <b>Restreindre l'accès cloud</b> reste cochée.

<b>Remarque</b> : Les adresses IP ou les ports fournis doivent correspondre à l'adresse IP externe que les serveurs {{site.data.keyword.SecureGateway}} verront, et non à l'adresse IP locale de la machine émettrice de la demande.

### Ajout de règles de table d'IP
Lors de l'ajout de règles dans des tables d'IP, vous devez indiquer des adresses IP individuelles ou une plage d'adresses IP ainsi qu'un port ou une plage de ports.  Toutes les plages fournies sont inclusives.  Le tableau suivant donne quelques exemples et montre comment les tables d'IP sont prises en compte :

Adresses IP | Ports | Résultat
-- | -- | --
1.2.3.4 | 5000 | Seule l'adresse IP 1.2.3.4 du port 5000 sera autorisée.
1.2.3.4-2.3.4.5 | 5000 | Toutes les adresses IP comprises entre 1.2.3.4 et 2.3.4.5 du port 5000 seront autorisées.
1.2.3.4 | 5000:10000 | Seule l'adresse IP 1.2.3.4 des ports 5000 à 10000 sera autorisée.
1.2.3.4-2.3.4.5 | 5000:10000 | Toutes les adresses IP comprises entre 1.2.3.4 et 2.3.4.5 des ports 5000 à 10000 seront autorisée.
1.2.3.4 | | Seule l'adresse IP 1.2.3.4 de n'importe quel port sera autorisée.
| 5000 | Toutes les adresses IP du port 5000 seront autorisées.

Des règles spécifiques peuvent également être associées à une application.  Pour plus d'informations sur la création de règles associées, voir la section consacrée à la [création de règles de table d'IP pour votre application](./iptables.html).

## Configuration d'options de proxy
Si votre destination sur site se trouve derrière un proxy SOCKS, vous pouvez configurer les paramètres de proxy de votre destination dans le panneau Options de proxy.
![Panneau Options de proxy](./images/proxyOptions.png?raw=true "Panneau Options de proxy")

Pour configurer les paramètres de proxy, il vous suffit d'indiquer le nom d'hôte et le port sur lesquels le proxy est en mode écoute ainsi que le protocole SOCKS utilisé (4, 4a, 5).

## Paramètres de la destination
Une fois votre destination créée, cliquez sur l'icône des paramètres pour afficher les informations suivantes :

- L'ID de destination requis pour utiliser l'API.
- <b>Les destinations sur site auront un numéro de port et un nom d'hôte de cloud.  Votre application a besoin de cette information pour se connecter à votre ressource sur site.</b>
- <b>Les destinations sur le cloud auront un port client.  Il s'agit du port sur lequel le client {{site.data.keyword.SecureGateway}} sera en mode écoute pour connecter à votre application sur site à votre ressource de cloud.</b>
- L'hôte et le port de ressource vers lesquels pointe cette destination.
- Quand la destination a été créée et quel utilisateur l'a créée.
- Quand la destination a été modifiée pour la dernière fois et quel utilisateur l'a modifiée.
