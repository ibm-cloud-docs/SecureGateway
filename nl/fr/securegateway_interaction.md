---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-19"

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Interaction avec le client
{: #client-interacting}

Différentes méthodes s'offrent à vous pour interagir avec le client :

- Via la ligne de commande du terminal avant le démarrage
- Après le démarrage, via la ligne de commande interactive du client
- Après le démarrage, via l'interface utilisateur locale du client

## Configurations de démarrage
{: #startup}

### Options et arguments de démarrage
{: #startup-args}

Le tableau suivant décrit toutes les options disponibles qui peuvent être associées à la commande de démarrage du client.  Ces options permettent de configurer totalement le client avant le démarrage plutôt que des opérations de configuration individuelles une fois le client en cours d'exécution.

| Paramètres et arguments | Description |
| ------------- | ----------- |
| &lt;ID_passerelle&gt; | Connexion à {{site.data.keyword.Bluemix_notm}} avec l'ID de passerelle fourni |
| -F, -\-aclfile &lt;file&gt; | Access control List file |
| -g, -\-gateway &lt;hostname:port&gt; | Used to manually select a specific gateway destination (advanced use only) |
| -l, -\-loglevel &lt;level&gt; | Change the log level to ERROR, INFO, DEBUG or TRACE |
| -p, -\-logpath &lt;file&gt; | Direct logging to a specific file |
| -t, -\-sectoken &lt;security token&gt; | The security token to use for this gateway connection |
| -P, -\-port &lt;port&gt; | The port for the UI to run on.  Defaults to port 9003 |
| -w, -\-password &lt;password&gt; | The password to protect the UI with.  Defaults to no password |
| -x, -\-proxy &lt;proxy agent&gt; | The proxy for the port 9000 connection |
| -\-noUI | Prevent the UI from starting up automatically |
| -\-allow | Allows all connections to the client. Is overridden by the ACL file, if provided |
| -\-service | After an initial connection, the parent will restart within 60s if all child clients are terminated |

<b>Remarque :</b> les indicateurs `--service`, `--allow` et `--noUI` doivent être les derniers paramètres des arguments de ligne de commande.

Le cas d'utilisation le plus basique consiste à démarrer avec une seule connexion client à l'aide des paramètres par défaut :

```
<ID-passerelle> -t <jeton_sécurité>
```
{: pre}

Ces options peuvent être étendues afin de prendre également en charge la connexion automatique de plusieurs clients.  Pour transmettre ces options à plusieurs connexions client, le transmission doit respecter un format spécifique.  Les ID de passerelle peuvent être transmis de la même manière que via une connexion client unique (en séparant les ID de passerelle par des espaces) ; toutefois, l'ordre de ces ID détermine l'ordre dans lequel le reste des arguments doivent suivre.  Lorsque vous transmettez les autres arguments, vous devez les séparer par deux traits d'union (`--`) pour les extraire correctement.  Si aucun argument n'es transmis pour un indicateur particulier, il est supposé de ne rien appliquer aux clients.

Si les arguments fournis ne sont pas suffisants pour satisfaire tous les ID de passerelle, ils seront appliqués dans l'ordre jusqu'à ce qu'il n'y ait plus d'arguments.  Par exemple, si deux ID de passerelle sont transmis et qu'un jeton de sécurité unique est transmis, le jeton sera appliqué au premier ID de passerelle et non au second.  

Si des ID de passerelle fournis requièrent des arguments différents, alors le mot clé `none` doit être indiqué à la place de tout argument particulier qui n'est pas imposé/fourni par une passerelle.  Par exemple, si trois ID de passerelle sont transmis et que l'utilisateur veut spécifier un niveau de journalisation pour la première et la troisième passerelle, l'argument se présente sous la forme `--loglevel DEBUG--none--TRACE`.  Dans ce cas, aucun argument n'aura la valeur par défaut INFO.

### Exemples d'argument de démarrage
{: #startup-examples}

Connexion client unique, niveau de journalisation personnalisé, pas d'interface utilisateur :

```
node lib/secgwclient.js <ID_passerelle> -t <jeton_sécurité> -l <loglevel> --noUI
```
{: pre}

Plusieurs connexions client, options par défaut :

```
node lib/secgwclient.js <ID_passerelle_1> <ID_passerelle_2> -t <jeton_sécurité_1>--<jeton_sécurité_2>
```
{: pre}

Plusieurs connexions client, tous les paramètres personnalisés :

```
node lib/secgwclient.js <monID_passerelle_1> <;monID_passerelle_2> -t none--<jeton pour passerelle 2> -l DEBUG--TRACE -p <chemin complet fichier journal passerelle 1>--<chemin complet fichier journal passerelle 2> -F <chemin complet fichier liste contrôle accès passerelle 1>
```
{: pre}

Retour à [Initiation - Ajout d'un client](/docs/services/SecureGateway/securegateway_client.html).

## Configuration interactive
{: #interactive}

### Commandes interactives
{: #interactive-commands}

Le client {{site.data.keyword.SecureGateway}} possède une interface de ligne de commande et une invite
shell qui facilitent la configuration et le contrôle. L'environnement interactif prend en charge des ensembles de fonctions plus riche que les arguments de ligne de commande ce qui facilite le contrôle interactif du client.

| Commandes interactives | Description       |
| ------------- | ----------- |
| A, acl allow &lt;nom d'hôte:port&gt; &lt;ID travailleur&gt; | Autorisation de la liste de contrôle d'accès |
| D, acl deny &lt;nom d'hôte:port&gt; &lt;ID travailleur&gt; | Refus de la liste de contrôle d'accès |
| N, no acl &lt;nom d'hôte:port&gt; &lt;ID travailleur&gt; | Suppression d'une entrée de la liste de contrôle d'accès |
| S, show acl &lt;ID travailleur&gt; | Affichage de toutes les entrées de la liste de contrôle d'accès |
| F, acl file &lt;file&gt; &lt;ID travailleur&gt; | Fichier de liste de contrôle d'accès |
| C, displayconfig &lt;ID travailleur&gt; | Affichage de la configuration {{site.data.keyword.SecureGateway}} en cours, si disponible |
| a, authorize &lt;ID travailleur&gt; | Active/désactive la substitution du paramètre rejectUnauthorized pour les connexions TLS sortantes pour le travailleur spécifié |
| t, sectoken &lt;jeton de sécurité&gt; | Jeton de sécurité à utiliser pour la connexion de passerelle suivante |
| c, connect &lt;ID passerelle&gt; | Connexion à {{site.data.keyword.Bluemix_notm}} avec l'ID de passerelle fourni |
| l, loglevel &lt;niveau&gt; &lt;ID travailleur&gt; | Définition du niveau de journalisation sur ERROR, INFO, DEBUG ou TRACE |
| p, logpath &lt;fichier&gt; &lt;ID travailleur&gt; | Envoi de la journalisation dans un fichier spécifique |
| r, reverse &lt;ID travailleur&gt; | Liste des ports sur lesquels le client est actuellement en mode écoute pour les destinations inverses |
| k, kill &lt;ID travailleur&gt;  | Arrêt du travailleur spécifié |
| e, select &lt;ID travailleur&gt;  | Indique un travailleur sur lequel exécuter des commandes, sauf indication contraire |
| d, deselect | Désélectionne le travailleur précédemment spécifié.  Exécutez la commande select pour en spécifier une autre. |
| w, password &lt;ancien mot de passe&gt; &lt;nouveau mot de passe&gt; | Définition des mots de passe d'interface utilisateur.  Si &lt;nouveau mot de passe&gt; est vide, aucun mot de passe n'est appliqué. &lt;ancien mot de passe&gt; est requis lors d'une mise à jour du mot de passe. Les mots de passe ne doivent contenir que des lettres |
| P, port &lt;nouveau port&gt; | Change le port sur lequel l'interface utilisateur écoute |
| u, uistart &lt;mot de passe initial&gt; &lt;port&gt; | Démarre l'interface utilisateur sur hôte local:&lt;port&gt;/tableau de bord. Si &lt;mot de passe initial&gt; est vide et qu'aucun autre mot de passe n'a été défini pour la session, aucun mot de passe d'interface utilisateur ne sera appliqué. Si &lt;port&gt; est vide, l'interface utilisateur sera accessible sur le port 9003 |
| U, uistop | Ferme l'interface utilisateur associée avec cette session client. La session sera accessible uniquement via l'interface de ligne de commande tant qu'une nouvelle interface utilisateur n'est pas démarrée manuellement |
| R, revoke | Efface toutes les autorisations d'interface utilisateur associées avec cette session client |
| q, quit | Déconnecter et quitter |
| s, status &lt;ID travailleur&gt;  | Affichage des détails du statut du tunnel et de ses connexions |
| L, list | Affiche la liste des travailleurs actuellement associés |

<b>Remarque :</b> si une connexion a été spécifiée avec la commande `select` et qu'une autre commande est appelée sans fournir d'ID travailleur, la commande tente de s'exécuter sur la connexion spécifiée par `select`.

Pour plus d'informations sur la configuration de la liste de contrôle d'accès, [cliquez ici](/docs/services/SecureGateway/securegateway_acl.html).

Retour à [Initiation - Ajout d'un client](/docs/services/SecureGateway/securegateway_client.html).

## Interface utilisateur client
{: #client-ui}

<b>Remarque :</b> l'interface utilisateur client n'est pas prise en charge lorsque vous utilisez Docker sous Windows ou MacOS.

L'interface utilisateur client fournit une interface Web locale qui permet à l'utilisateur d'interagir avec le client {{site.data.keyword.SecureGateway}} au lieu de l'interface de ligne de commande.  Par défaut, cette interface utilisateur est disponible sur `localhost:9003/dashboard`. L'interface utilisateur est divisée en plusieurs pages :

### Connexion
{: #ui-connect}

Il s'agit de la page initiale de démarrage pour l'interface utilisateur dans laquelle l'utilisateur peut fournir un ID de passerelle et un jeton de sécurité pour connecter son premier client.

### Identification
{: #ui-login}

Cette page s'affiche si l'interface utilisateur est protégée par mot de passe.  Si elle s'affiche alors qu'aucun mot de passe n'est appliqué, actualisez la page pour être redirigé.

### Tableau de bord
{: #ui-dashboard}

Il s'agit de la page principale après connexion d'un client.  A partir de cette page, vous pouvez accéder à la page Affichage des journaux, à la page Liste de contrôle d'accès et à la page Informations de connexion.  Au bas de la page, vous pouvez également choisir de déconnecter un ou plusieurs des clients connectés.  En haut de la page, le client actuellement sélectionné s'affiche, ainsi qu'une option pour se connecter à d'autres clients. 

### Affichage des journaux
{: #ui-logs}

Cette page vous permet de visualiser les journaux générés par le client sélectionné (indiqué en haut à droite de la page).  Les journaux affichés peuvent être filtrés via les cases à cocher qui se trouvent dessous.

### Liste de contrôle d'accès
{: #ui-acl}

Cette page vous permet de liste de contrôle d'accès la liste de contrôle d'accès du client sélectionné (indiqué en haut à droite de la page).  Des règles peuvent être ajoutées individuellement aux tables d'autorisation/refus ou vous pouvez télécharger un fichier à l'aide d'une option du bas de la page.

### Informations de connexion
{: #ui-info}

Cette page affiche des informations sur la connexion en cours pour le client sélectionné (indiqué en haut à droite de la page).  Des informations comme la description de la passerelle, le nombre de connexions actuelles ou les programmes d'écoute des destinations inverses peuvent être visualisées ici.

Retour à [Initiation - Ajout d'un client](/docs/services/SecureGateway/securegateway_client.html).

## Arrêt du client distant
{: #client-remote}

Si un client est associé à un ID, vous pouvez l'arrêter à distance à l'aide de l'interface utilisateur SG ou à l'aide de l'API SG.  Si vous arrêtez un client qui s'exécute en tant que service, le client redémarrera et obtiendra un nouvel ID client ; cependant, si le client a plusieurs clients connectés, le client arrêté ne redémarrera pas tant que tous les autres clients n'auront pas été arrêtés.

## Limitations
{: #limits}

### Limites de connexion
{: #limits-conn}

La passerelle SG peut gérer au maximum 250 connexions simultanées. Si le nombre de demandes simultanées excède cette limite, cela peut entraîner un rejet des tentatives de connexion et engendrer des temps d'attente. Un moyen simple pour résoudre ce problème consiste à utiliser un regroupement de connexions sur l'application appelante. Sachez que la limite de 250 connections simultanées s'applique à la passerelle et non au client ou à la destination. Cette limite se partage entre tous les clients et les destinations sur la passerelle.

### Limites applicable au client DataPower
{: #limits-datapower}

Le client DataPower {{site.data.keyword.SecureGateway}} va être mis à jour pour intégrer les fonctions de nos autres clients.  Il présente actuellement les limitations suivantes :

- ACL prendra par défaut la valeur ALLOW ALL.
- L'activation et la désactivation de passerelles ou de destinations depuis l'interface utilisateur de
{{site.data.keyword.SecureGateway}} n'est pas prise en charge. Toutefois, l'option Administrative State dans l'interface utilisateur de DataPower opère comme un commutateur activé/désactivé pour ce client spécifique.
- L'interrogation du statut de connexion pour les mises à jour en temps réel du statut de passerelles connectées et déconnectées n'est pas
prise en charge.
- Les chaînes de certificats complètes avec TLS côté destination ne sont pas prises en charge avant la version 7.5.1.0 de DataPower.
- Les destinations dans le cloud ne sont pas prises en charge avant la version 7.5.1.0 de DataPower.
- Le niveau de journalisation ne peut pas être défini sur TRACE.
- La version Secure Gateway Client la plus récente dans DataPower est 1.8.0fp6. Cliquez [ici](/docs/services/SecureGateway/securegateway_install.html#installing-datapower) pour plus d'informations. 
