---

copyright:
  years: 2015, 2018
lastupdated: "2018-10-11"

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Installation du client

## Docker
{: #docker}

Docker est une plateforme tierce qui propose une approche basée sur un conteneur pour une installation facile et rapide des applications avec
peu ou pas de configuration. Le service {{site.data.keyword.SecureGateway}} fournit une image Docker à utiliser après l'installation de l'utilitaire Docker sur votre poste de travail.  Pour installer Docker, accédez au site Web d'installation de Docker et suivez les instructions concernant votre système.

### Installation et mise à jour du client
{: #docker-install}

L'installation et la mise à jour de l'image Docker du client {{site.data.keyword.SecureGateway}} utilisent toutes deux la même commande :

```
docker pull ibmcom/secure-gateway-client
```
{: pre}

### Démarrage d'une session client interactive
{: #docker-run}

Pour exécuter le conteneur Docker pour IBM {{site.data.keyword.SecureGateway}}, exécutez la commande suivante :

```
docker run -it ibmcom/secure-gateway-client <ID passerelle> -t <jeton de sécurité>
```
{: pre}

En général, elle contient deux paramètres, votre ID de passerelle {{site.data.keyword.SecureGateway}} et le jeton de sécurité de la passerelle, tous deux disponibles via le tableau de bord {{site.data.keyword.SecureGateway}}.

### Commandes Docker prises en charge
{: #docker-commands}

Le client {{site.data.keyword.SecureGateway}} prend uniquement en charge les commandes `pull` et `run` pour la manipulation du conteneur.

Retour à [Initiation - Ajout d'un client](/docs/services/SecureGateway/securegateway_client.html).

## Mac OS X
{: #mac}

### Exigences pour une exécution sur Mac OS X
{: #mac-requirements}

Les conditions préalables suivantes doivent être remplies pour une exécution du client sur Mac OS X.
 1. Installez les outils de développement Xcode, nécessaires à certains des modules de noeud sur cette plateforme.

### Procédure d'installation de Mac OS X
{: #mac-install}

Selon la configuration de sécurité de votre système, vous devez disposer des privilèges d'administration pour effectuer cette installation.

 1. Montez l'image DMG téléchargée depuis l'interface utilisateur {{site.data.keyword.SecureGateway}}, en général en cliquant deux fois sur l'image.
 2. Une nouvelle fenêtre d'outil de recherche doit s'afficher.  Cette fenêtre doit contenir un raccourci Applications sur lequel vous faites glisser et déposez l'application.  Si elle ne contient pas ce raccourci, cliquez deux fois sur le volume monté puis faites glisser et déposez l'icône d'application sur l'icône Applications dans la barre de navigation de l'outil de recherche.

### Démarrage d'une session client interactive
{: #mac-run}

Pour démarrer le client, exécutez le fichier `secgw.command` qui se trouve dans le répertoire d'installation par défaut : `/Applications/ibm/`.

Retour à [Initiation - Ajout d'un client](/docs/services/SecureGateway/securegateway_client.html).

## Linux
{: #linux}

L'installation inclut le client {{site.data.keyword.SecureGatewayfull}} ainsi qu'une version sécurisée du package nodejs d'IBM.  Les deux sont installés dans le répertoire /opt/ibm sur le système.  Le programme d'installation crée ou met à jour les éléments suivants :

| Répertoire | Nom de fichier | Description          |
| ------------- | ------------- | ----------- |
| /etc/ibm | sgenvironment.conf | Fichier de configuration de upstart |
| /etc/ibm | passwd | Crée l'utilisateur : secgwadmin:x:501:501::/home/secgwadmin:/bin/bash |
| /etc/ibm | group | Crée le groupe : secgwadmin:x:501: |
| /etc/init | securegateway_client.conf | Fichier upstart |
| /opt/ibm | node-v&lt;version&gt;-linux-x64 | Installation du package Node JS d'IBM |
| /opt/ibm | securegateway | Installation du client {{site.data.keyword.SecureGateway}} |
| /usr/local/bin | node | symlink créé pour l'exécutable du noeud d'IBM |
| /usr/local/bin | npm | symlink créé pour l'exécutable npm d'IBM |
| /var/log/securegateway | client_console.log | Fichier journal créé lors de l'exécution du client en tant que processus de démarrage automatique |

Vous devez disposer des privilèges d'administration ou de superutilisateur pour effectuer cette installation.

### Installation Ubuntu/PowerPC
{: #debian-install}

 1. Installez le client {{site.data.keyword.SecureGateway}}. Par exemple, si vous utilisez un package Debian, entrez la commande suivante :

    ```
    sudo dpkg -i <secure-gateway-debian-package>
    ```
    {: pre}

 2. Lorsque le programme d'installation du client démarre, vous êtes invité à entrer les informations suivantes.

    <b>Démarrer automatiquement le processus Oui/Non</b>
       Si vous procédez à une mise à niveau ou à une réinstallation, vous devez décider si le client existant doit être arrêté pendant
        l'exécution du processus d'installation. Sélectionnez O/o pour arrêter le client existant. Sinon, le package client
        est mis à niveau sans redémarrage du client, ce qui signifie que vous pouvez attendre une fenêtre de mise à jour logicielle appropriée
        pour effectuer un redémarrage. Si vous sélectionnez N/n, l'installation se poursuit et vous devez redémarrer le client
        manuellement.

    <b>ID de passerelle</b>
        Définissez l'ID de passerelle pour le client. Il est défini lorsque vous créez un service {{site.data.keyword.SecureGateway}}. Si le
        client ne parvient pas à se connecter, vous pouvez changer votre sélection en éditant le fichier de configuration.

    <b>Jeton de sécurité</b>
        Si l'ID de passerelle est activé pour vérifier un jeton de sécurité, vous devez fournir le jeton de sécurité maintenant. S'il ne requiert pas de jeton de
sécurité, laissez cette zone vide.

    <b>Niveau de journalisation</b>
        Le paramètre par défaut est INFO. Les autres valeurs admises sont TRACE, DEBUG et ERROR.

    <b>Liste de contrôle d'accès</b>
        Entrez le chemin d'accès absolu au fichier qui contient la liste de contrôle de l'accès à vos
        ressources sur site. Pour plus d'informations, voir le fichier README.md dans le répertoire /opt/ibm/securegateway ou la section Liste de contrôle d'accès.

    <b>Interface utilisateur client</b>
        Indiquez si vous voulez utiliser l'interface utilisateur du client. Dans l'affirmative, l'utilisateur peut modifier le port de l'interface utilisateur. La valeur par défaut est 9003.

    <b>Remarque :</b> vous n'êtes pas obligé de répondre à toutes les invites ; en l'absence de réponse, elles prendront la valeur par défaut ou resteront vides dans le fichier de configuration de l'environnement. Ainsi, le processus d'installation peut s'exécuter sans intervention de l'utilisateur.

    <b>Remarque :</b> le fichier sgenvironment.conf est lu à chaque fois que vous démarrez ou redémarrez le client avec le processus upstart du système. Vous pouvez éditer le fichier /etc/ibm/sgenvironment.conf à tout moment pour changer votre configuration et redémarrer le client pour qu'il applique les modifications.

    <b>Remarque :</b> la langue des journaux du service client {{site.data.keyword.SecureGateway}} peut être modifiée en changeant le paramètre `LANGUAGE` du fichier /etc/ibm/sgenvironment.conf. Les journaux du service adopteront la langue sélectionnée une fois le service redémarré.

 3. Si vous avez redémarré le client, pour vérifier qu'il s'exécute correctement, entrez la commande suivante :

    ```
    cat /var/log/securegateway/client_console.log
    ```
    {: pre}

 4. Si vous voulez éditer le fichier de configuration, pour vérifier ensuite qu'il a été correctement mis à jour, entrez la commande suivante :

    ```
    sudo vi /etc/ibm/sgenvironment.conf
    ```
    {: pre}

 5. Pour vérifier le statut de votre installation client, entrez la commande suivante :

    ```
    sudo dpkg --status ibm-securegateway-client
    ```
    {: pre}

Exemple de sortie :

```
Package: ibm-securegateway-client
Status: install ok installed
Priority: extra
Section: IBM/net
Maintainer: cloudoe-dev@webconf.ibm.com
Architecture: amd64
Source: ibm-securegateway+securegateway-client
Version: 1.7.0
Description: IBM Secure Gateway Client for Bluemix
IBM Secure Gateway client package for Bluemix, supports secure connections to on-premises connections.
Homepage: https://ng.bluemix.net/
License: http://www.ibm.com/software/sla/sladb.nsf/lilookup/986C7686F22D4D3585257E13004EA6CB?OpenDocument
```
{: screen}

### Installation RedHat/SuSE
{: #rpm-install}

1. Installez le client {{site.data.keyword.SecureGateway}}. Par exemple, si vous utilisez un package RPM, entrez la commande suivante :

   ```
   rpm -ivhf <secure-gateway-rpm-package>
   ```
   {: pre}


   Le programme d'installation du client démarre et installe le client ; il crée un fichier sgenvironment.conf dans /etc/ibm.

2. Facultatif : si vous voulez utiliser le processus upstart du système, vous devez éditer ce fichier et fournir les informations suivantes pour que le client démarre correctement. Pour plus d'informations sur l'édition de ce fichier de configuration, voir [Utilisation d'upstart](/docs/services/SecureGateway/securegateway_auto-start.html#linux).

3. Si vous avez démarré le client à l'aide de upstart, consultez le fichier journal pour vous assurer qu'il fonctionne correctement.

   ```
   cat /var/log/securegateway/client_console.log
   ```
   {: pre}

4. Pour vérifier le statut de votre installation client, entrez la commande suivante :

   ```
   rpm -q ibm-securegateway-client
   ```
   {: pre}

### Démarrage d'une session client interactive
{: #linux-run}

Pour démarrer le client, exécutez la commande suivante :

```
cd /opt/ibm/securegateway/client
node lib/secgwclient.js <ID passerelle> -t <jeton de sécurité>
```
{: codeblock}

En général, elle contient deux paramètres, un ID de passerelle {{site.data.keyword.SecureGateway}} et le jeton de sécurité de la passerelle, tous deux disponibles via le tableau de bord {{site.data.keyword.SecureGateway}}.

Retour à [Initiation - Ajout d'un client](/docs/services/SecureGateway/securegateway_client.html).

## Windows
{: #windows}

### Installation du client
{: #windows-install}

Selon la configuration de sécurité de votre système, vous devez disposer des privilèges d'administration pour effectuer cette installation.

 1. Copiez le fichier du programme d'installation EXE sur votre système.  Une somme MD5 est également fournie. Elle vous permet de vérifier
l'intégrité du package d'installation.

 2. Procédez à l'installation soit en cliquant deux fois dessus et en répondant aux invites, soit en exécutant l'exécutable depuis l'invite de commande.

 3. L'utilisateur est invité à sélectionner le répertoire d'installation qui lui convient. L'emplacement par défaut est le répertoire Program Files (x86).

 4. L'utilisateur est invité à sélectionner la langue de l'interface de ligne de commande. Si aucune langue n'est sélectionnée, l'interface s'affiche par défaut en anglais.

 5. L'utilisateur est invité à installer le client en tant que service Windows. S'il valide ce choix, le client s'exécute en arrière-plan en tant que service Windows avec la configuration indiquée dans la boîte de dialogue suivante. Vous pouvez vérifier le statut du service dans la page Service du Panneau de configuration. Le service se nomme "IBM Bluemix Secure Gateway Service".

 6. Le programme d'installation identifie s'il existe une installation sur la machine. Si tel est le cas, l'utilisateur doit indiquer s'il veut utiliser les configurations existantes ou entrer de nouvelles configurations. Les informations, telles que les ID de passerelle, les jetons de sécurité, les fichiers de liste de contrôle d'accès et les niveaux de journalisation de chaque passerelle, peuvent être entrées ici. Si l'utilisateur choisit d'exécuter le client en tant que service Windows, le client démarre avec les configurations fournies. Si l'utilisateur n'a pas choisi d'exécuter le client en tant que service Windows, les configurations sont enregistrées pour une utilisation ultérieure. Dans les deux cas, les configurations sont enregistrées dans %répertoire_installation%/ibm/securegateway/client/securegw_service.config.

 7. A compter de la version 1.5.0, le client est livré avec une interface utilisateur client totalement fonctionnelle. Elle est configurable à partir du programme d'installation. L'utilisateur peut fournir le mot de passe et le numéro du port (par défaut 9003) à partir duquel l'interface utilisateur du client sera lancée.

 Si l'utilisateur choisit de lancer le client avec une connexion à plusieurs passerelles, faites attention en fournissant les valeurs de configuration. Les ID de passerelle doivent être séparés par des espaces. Les jetons de sécurité, fichiers de liste de contrôle d'accès et niveaux de journalisation doivent être délimités par --. Si vous ne souhaitez pas fournir certaines de ces valeurs pour une passerelle particulière, entrez la valeur 'none'.

 <b>Remarque :</b> vérifiez qu'il n'y a pas d'espaces blancs résiduels.

 <b>Remarque :</b> notez que les fichiers de liste de contrôle d'accès doivent être placés dans le répertoire %répertoire_installation%/ibm/securegateway/client ou dans un sous-répertoire de ce répertoire.

### Démarrage d'une session client interactive
{: #windows-run}

Pour démarrer le client, ouvrez une invite de commande avec des privilèges d'administration et exécutez la commande suivante :

```
cd "<répertoire_installation>\ibm\securegateway\client"
secgw.cmd
```
{: codeblock}

Vous pouvez également accéder à `<Installation_directory>\ibm\securegateway\client`, puis cliquer deux fois sur `secgw.cmd`.

<b>Remarque :</b> vous pouvez choisir d'utiliser les configurations enregistrées dans le fichier `<Installation_directory>\ibm\securegateway\client\securegw_service.config` ou fournir les détails interactivement.

Retour à [Initiation - Ajout d'un client](/docs/services/SecureGateway/securegateway_client.html).

## DataPower
{: #datapower}

DataPower dispose d'une version intégrée du client {{site.data.keyword.SecureGateway}}.  La version du client {{site.data.keyword.SecureGateway}} dont vous disposez dépend de la version de DataPower.  Tenez compte des éventuelles [limitations du client DataPower](/docs/services/SecureGateway/securegateway_interaction.html#limits-datapower) applicables. L'utilisation de l'ancien client Secure Gateway peut engendrer des erreurs inattendues.

| Version de DataPower | Version du client {{site.data.keyword.SecureGateway}}  |
| -- | --  |
| 7.2.0.0, 7.5.0.0 | 1.1.0  |
| 7.5.1.0, 7.7.0 | 1.4.2  |
| 7.5.2.4 | 1.6.1  |
| 7.5.2.6, 7.6.0.0 | 1.7.0  |
| 7.5.2.14, 7.6.0.7, 7.7.1.0 |  1.8.0fp6  |

### Démarrage d'une session client
{: #datapower-run}

1. Connectez-vous à l'interface Web de DataPower.
2. Accédez à Objects -> Cloud -> Secure Gateway Client ou rechercher Secure Gateway Client.
3. Cliquez sur `Add` pour configurer une nouvelle connexion client.
4. Entrez un nom, l'ID de passerelle et le jeton de sécurité (le cas échéant) et appliquez les modifications.

Retour à [Initiation - Ajout d'un client](/docs/services/SecureGateway/securegateway_client.html).
