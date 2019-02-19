---

copyright:
  years: 2015, 2017
lastupdated: "2017-04-25"

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Configuration du démarrage automatique pour le client

## Linux
{: #linux}

Si vous avez opté pour la fonction de démarrage automatique de votre système, utilisez l'une des méthodes suivantes pour démarrer le client.  Le fichier de configuration qu'utilisera la client est disponible dans :

```
/etc/ibm/sgenvironment.conf
```
{: screen}

<b>Remarque :</b> la fonction de démarrage automatique n'est pas disponible pour Red Hat Linux 6.

Ce fichier contient les variables importantes suivantes à définir :

| Variable d'environnement | Description       |
| ------------- | ----------- |
| RESTART_CLIENT | Arrêter et redémarrer le client lors de l'installation ou de la mise à niveau (Yes ou No) |
| GATEWAY_ID | ID de passerelle, tel qu'il a été créé dans l'interface utilisateur Secure Gateway for Bluemix |
| SECTOKEN | Jeton de sécurité pour cet ID de passerelle, si vous avez choisi d'appliquer la sécurité lors de sa création |
| ACL_FILE | Fichier de liste de contrôle d'accès à utiliser pour restreindre l'accès sur site aux ressources |
| LOGLEVEL | Niveau de journalisation que vous voulez appliquer pour votre service (par défaut, INFO) |
| USE_UI   | Définissez cette variable sur 'N' si vous ne voulez pas lancer l'interface utilisateur du client |
| UI_PORT  | Port sur lequel vous voulez lancer l'interface utilisateur du client (par défaut, le port 9003) |
| LANGUAGE | Langue des journaux du client (par défaut, en anglais) |

<b>Remarque :</b> ce fichier est en lecture seule si vous utilisez la fonction de démarrage automatique de votre système.  Si vous exécutez le client manuellement, ce fichier est ignoré.

### Upstart
{: #upstart}

### Démarrage du client
{: #upstart-start}

Si vous utilisez la fonction upstart permettant au client Secure Gateway de démarrer automatiquement au démarrage du système, vous devez d'abord vérifier le fichier de configuration du client (/etc/ibm/sgenvironment.conf).  Une fois le service upstart configuré, vous pouvez utiliser la commande ci-après pour le démarrer :

```
sudo initctl start securegateway_client
```
{: pre}

Pour redémarrer le service :

```
sudo initctl restart securegateway_client
```
{: pre}

### Arrêt du client
{: #upstart-stop}

Pour arrêter le service, exécutez la commande suivante :

```
sudo initctl stop securegateway_client
```
{: pre}

### SystemD
{: #systemd}


### Démarrage du client
{: #systemd-start}

Si vous utilisez la fonction systemD permettant au client Secure Gateway de démarrer automatiquement au démarrage du système, vous devez d'abord vérifier le fichier de configuration du client (/etc/ibm/sgenvironment.conf).  Une fois le service upstart configuré, vous pouvez utiliser la commande ci-après pour le démarrer :

```
systemctl start securegateway_client
```
{: pre}

Pour redémarrer le service :

```
systemctl restart securegateway_client
```
{: pre}

Pour le statut sur le service :

```
systemctl status securegateway_client
```
{: pre}

### Arrêt du client
{: #systemd-stop}

Pour arrêter le service, exécutez la commande suivante :

```
systemctl stop securegateway_client
```
{: pre}

### SystemV
{: #systemv}

System V n'est pas configuré lors de l'installation comme les autres fonctions de démarrage automatique. Les scripts suivants sont disponibles dans le répertoire d'installation /opt/ibm/securegateway/client/upstart :

```
secgw.sh
securegateway_clientd
```
{: screen}

<b>Remarque :</b> cette section concerne les utilisateurs de SuSE/SLES 11.

Facultatif : si vous exécutez SuSE version 11 qui utilise la fonction de démarrage automatique systemV, des scripts permettant de configurer ce processus sont fournis. Suivez la procédure ci-dessous :

```
cd /opt/ibm/securegateway/client/upstart/systemV
cp secgw.sh /usr/local/bin
chmod +x /usr/local/bin/secgw.sh
cp securegateway_clientd /usr/local/bin
chmod +x /usr/local/bin/securegateway_clientd
cd /etc/init.d
ln -s /usr/local/bin/securegateway_clientd
insserv securegateway_clientd
vi /etc/ibm/sgenvironment.conf
```
{: codeblock}

Une fois cette procédure exécutée, les commandes YasT et systemV peuvent être utilisées pour démarrer et arrêter le démon.

Retour à [Initiation - Ajout d'un client](/docs/services/SecureGateway/securegateway_client.html).

## Windows
{: #windows}

Pour modifier l'état du service Windows, ouvrez une fenêtre de commande avec des droits d'administrateur.

```
cd "<répertoire_installation>\ibm\securegateway\client"
```
{: pre}

Si le service s'exécute déjà, vous pouvez l'arrêter avec la commande suivante :

```
windowsService.cmd uninstall
```
{: pre}

Pour démarrer le service, exécutez la commande suivante :

```
windowsService.cmd install
```
{: pre}

<b>Remarque :</b> le service s'exécutera en fonction des configurations stockées dans :

```
%répertoire_installation%/ibm/securegateway/client/securegw_service.config
```
{: pre}

Les journaux d'application du service Windows sont enregistrés dans :

```
%répertoire_installation%/ibm/securegateway/client/logs/securegw_win_service.log
```
{:pre}

 Ces journaux sont enregistrés chaque jour dans un nouveau fichier.

Retour à [Initiation - Ajout d'un client](/docs/services/SecureGateway/securegateway_client.html).
