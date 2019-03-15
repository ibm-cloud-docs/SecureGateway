---

copyright:
  years: 2015, 2017
lastupdated: "2017-04-10"

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Liste de contrôle d'accès
{: #acl}

Le client {{site.data.keyword.SecureGateway}} offre une prise en charge intégrée des listes de contrôle. Vous pouvez autoriser ou interdire (refuser) l'accès aux ressources sur site en modifiant la liste de contrôle d'accès du client.  Cette opération peut s'effectuer de manière interactive à l'aide de commandes du client ou en spécifiant un fichier qui contient les listes de contrôle d'accès que vous voulez appliquer.

A compter de la version 1.5.0, des règles de liste de contrôle d'accès seront synchronisées sur tous les clients connectés à la même passerelle.  Ainsi, il vous suffit d'établir ou de mettre à jour votre liste de contrôle d'accès depuis un client unique et elle est partagée sur tous les clients en cours d'exécution connectés à cette passerelle.  La liste de contrôle d'accès sera également conservée entre les sessions, de sorte que la connexion à un nouveau client appliquera les mêmes règles de liste de contrôle d'accès.

## Commandes de liste de contrôle d'accès
{: #acl-commands}

Les commandes de liste de contrôle d'accès prises en charge sont les suivantes :

```
acl allow [<nom d'hôte>]:[<port>]
acl deny [<nom d'hôte>]:[<port>]
no acl [<nom d'hôte>]:[<port>]
no acl
show acl
```
{: screen}

Lorsque vous omettez le nom d'hôte ou le port, tous les noms d'hôte ou ports sont concernés.  Par exemple, voici une règle de liste de contrôle d'accès qui autorise tous les noms d'hôte du port 22.

```
acl allow :22
```
{: pre}

Voici un autre exemple de règle de liste de contrôle d'accès qui autorise tous les noms d'hôte de tous les ports, ce qui désactive la prise en charge des liste de contrôle d'accès. <b>Cela n'est pas recommandé.</b>

```
acl allow :
```
{: pre}

La commande `show acl` affiche la liste de contrôle d'accès actuellement définie ou un message relatif au paramétrage global.

Retour à [Initiation - Ajout d'un client](/docs/services/SecureGateway/securegateway_client.html).

## Contrôle de route HTTP/S à l'aide d'une liste de contrôle d'accès
{: #acl-route-control}

A compter de la version 1.6.0, les destinations HTTP/S peuvent aussi imposer des routes spécifiques sur les entrées de liste de contrôle d'accès.  Celles-ci sont ajoutées de la même façon que les entrées de liste de contrôle d'accès classiques mais avec le chemin inclus à la fin de la règle. Par exemple, la commande suivante autorise uniquement le passage des demandes qui suivent le chemin /my/api :

```
acl allow localhost:80/my/api
```
{: pre}

Une fois cette règle définie, les demandes via `<cloud host>:<cloud port>/my/api*` seront autorisées.

Les routes ne sont prises en charge que sur les commandes `acl allow`.

## Priorité des listes de contrôle d'accès
{: #acl-precedence}

Une fois la commande `acl allow :` fournie, si d'autres commandes `acl allow` sont entrées, la règle d'autorisation `ALL:ALL` (provenant d'`acl allow :`) est retirée de la liste car il est supposé que vous ne voulez plus autoriser l'accès illimité.  Après avoir fourni une commande `acl deny :`, si une autre commande `acl deny` est entrée, la règle de refus `ALL:ALL` (provenant d'`acl deny :`) est retirée de la liste car il est supposé que vous ne voulez plus restreindre tous les accès.  Si vous affichez vos règles de liste de contrôle d'accès actuelles via la commande `show acl` de l'interface de ligne de commande, un indicateur signale si les règles non répertoriées sont autorisées ou refusées.

## Importation d'un fichier de liste de contrôle d'accès
{: #import-acl-file}

Vous pouvez indiquer dans la commande `acl file` le nom d'un fichier qui contient les commandes de liste de contrôle d'accès prises en charge que le client lira au démarrage. Les commandes que contient ce fichier doivent être au format suivant :

```
acl allow [<nom d'hôte>]:[<port>]
acl deny [<nom d'hôte>]:[<port>]
no acl [<nom d'hôte>]:[<port>]
no acl
```
{: screen}

<b>Remarque :</b> `no acl` sans autres paramètres réinitialise la table LCA et définit l'accès à DENY ALL.

Pour un exemple de fichier de liste de contrôle d'accès, cliquez [ici](/docs/services/SecureGateway/securegateway_acl-file.html).

Retour à [Initiation - Ajout d'un client](/docs/services/SecureGateway/securegateway_client.html).

## Copie de votre fichier LCA dans le client {{site.data.keyword.SecureGateway}} Docker
{: #copy-acl-to-docker}

Le client Docker {{site.data.keyword.SecureGateway}} s'exécute essentiellement dans son propre conteneur de virtualisation.  Par conséquent, le système de fichiers de la machine hôte n'est pas directement accessible pour les processus qui s'exécutent dans le conteneur, y compris le client {{site.data.keyword.SecureGateway}}.  A compter de la version 1.8.0 de Docker Engine, vous pouvez utiliser la commande 'docker cp' pour importer les fichiers qui existent sur votre machine hôte dans le conteneur, qu'il soit en cours d'exécution ou arrêté, afin de pouvoir utiliser la commande interactive ACL FILE du client {{site.data.keyword.SecureGateway}}.

Pour utiliser la prise en charge 'cp' interactive dans Docker depuis votre hôte vers l'instance Docker, vous devez disposer de Docker version 1.8.0. Pour le vérifier, exécutez la commande `docker --version`

Une fois cette opération terminée, votre version doit s'afficher comme suit. Sachant qu'il est recommandé d'autoriser Docker à s'exécuter en tant qu'utilisateur non root, exécutez la commande suggérée après avoir procédé à la mise à niveau de votre moteur vers la version 1.8.0 ou 1.8.2.

```
Client:
 Version:      1.8.2
 API version:  1.20
 Go version:   go1.4.2
 Git commit:   0a8c2e3
 Built:        Thu Sep 10 19:21:21 UTC 2015
 OS/Arch:      linux/amd64

Server:
 Version:      1.8.2
 API version:  1.20
 Go version:   go1.4.2
 Git commit:   0a8c2e3
 Built:        Thu Sep 10 19:21:21 UTC 2015
 OS/Arch:      linux/amd64
```
{: screen}

Pour importer votre liste de fichiers acl dans l'image Docker, procédez comme suit :

- Exécutez la commande 'docker ps' pour trouver votre ID conteneur

```
CONTAINER ID IMAGE                        COMMAND                CREATED        STATUS  PORTS NAMES
764aadce386b ibmcom/secure-gateway-client "node lib/secgwclient" 27 seconds ago Up 26 seconds condescending_nobel
```
{: screen}

- Copiez acl.list à l'aide de la commande 'docker cp' en utilisant le nom ou l'ID de conteneur :

```
docker cp 01_client.list 764aadce386b:/root/01_client.list
```
{: pre}

- Puis, dans le client {{site.data.keyword.SecureGateway}} s'exécutant dans Docker :

```
cli F /root/01_client.list

 [2015-10-01 08:12:30.091] [INFO] The current access control list is being reset and replaced by the user provided file: /root/01_client.list
 [2015-10-01 08:12:30.093] [INFO] The ACL file process accepts acl allow 127.0.0.1:27017
 [2015-10-01 08:12:30.094] [INFO] The ACL file process accepts acl allow 127.0.0.1:22
```
{: screen}

- Affichez la liste de contrôle d'accès :

```
cli> S
 -------------------------------------------------------------------
           -- Secure Gateway Client Access Control List --          

  hostname                               port                  value
  127.0.0.1                             27017                  Allow
  127.0.0.1                                22                  Allow
 -------------------------------------------------------------------
```
{: screen}

Retour à [Initiation - Ajout d'un client](/docs/services/SecureGateway/securegateway_client.html).
