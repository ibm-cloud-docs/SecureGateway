---

copyright:
  years: 2015, 2017
lastupdated: "2017-04-25"

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Désinstallation du client
{: #client-uninstall}

## Docker
{: #docker-uninstall}

Pour supprimer l'image Docker du client {{site.data.keyword.SecureGateway}}, exécutez la commande suivante :

```
docker rmi ibmcom/secure-gateway-client
```
{: pre}

## Mac OS X
{: #mac-uninstall}

Pour supprimer de Mac OS X le client {{site.data.keyword.SecureGateway}}, exécutez les commandes suivantes à partir du terminal :

```
cd /Applications
rm -rf ./ibm
```
{: codeblock}

## Linux
{: #linux-uninstall}

### Ubuntu/PowerPC
{: #debian-uninstall}

Pour déterminer le statut de l'installation du client {{site.data.keyword.SecureGateway}}, vous pouvez
utiliser la commande suivante :

```
sudo dpkg -l | grep ibm-securegateway-client
```
{: pre}

Pour désinstaller le client {{site.data.keyword.SecureGateway}} sans supprimer le répertoire d'installation, les fichiers de configuration, l'ID, l'ID de groupe
et les fichiers journaux, exécutez la commande suivante :

```
sudo dpkg -r ibm-securegateway-client
```
{: pre}

Pour désinstaller le client {{site.data.keyword.SecureGateway}} et supprimer tous les éléments, exécutez la commande suivante :

```
sudo dpkg --purge ibm-securegateway-client
```
{: pre}

### RedHat/SuSE
{: #rpm-uninstall}

Pour déterminer le statut de l'installation du client {{site.data.keyword.SecureGateway}}, vous pouvez
utiliser la commande suivante :

```
rpm -qa | grep ibm-securegateway-client
```
{: pre}

Pour désinstaller le client {{site.data.keyword.SecureGateway}} sans supprimer le répertoire d'installation, les fichiers de configuration, l'ID, l'ID de groupe
et les fichiers journaux, exécutez la commande suivante :

```
rpm -e ibm-securegateway-client
```
{: pre}

Pour désinstaller le client {{site.data.keyword.SecureGateway}} et supprimer tous les éléments, exécutez la commande suivante :

```
yum remove ibm-securegateway-client
```
{: pre}

## Windows
{: #windows-uninstall}

Deux méthodes permettent de supprimer l'installation Windows du client {{site.data.keyword.SecureGateway}} :

1. Suppression depuis le répertoire d'installation : accédez au répertoire d'installation du client {{site.data.keyword.SecureGateway}}, puis cliquez deux fois sur uninstall.exe.

2. Suppression depuis le panneau de configuration : supprimez l'application {{site.data.keyword.SecureGateway}} de la section Programmes du panneau de configuration.
