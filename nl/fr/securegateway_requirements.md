---

copyright:
  years: 2015, 2018
lastupdated: "2018-08-10"

---

# Exigences pour exécuter le client

## Configuration système requise
{: #system}

Le client Secure Gateway est pris en charge dans les environnements suivants :

| Nom | Versions          |
| ------------- | ----------- |
| Windows Desktop | 7, 8.1, 10 et versions ultérieures |
| Windows Server | 2012 R2 et versions ultérieures |
| Red Hat Linux | 6.5 et versions ultérieures |
| CentOS | 7.2 et versions ultérieures |
| SuSE Linux | 11.0<sup>*</sup> et versions ultérieures |
| Ubuntu Linux | 14.04 (LTS) et versions ultérieures |
| Power Machine | architecture ppc64el |
| Ubuntu Z-Linux | - |
| AIX | 7.1 et versions ultérieures |
| Mac OS X | 10.10 (Yosemite) et versions ultérieures |
| Docker | 1.7.0 et versions ultérieures, tous les systèmes d'exploitation pris en charge |

<sup> * </sup>- SLES 11 est pris en charge uniquement avec le service pack 4

<b>Remarque :</b> seuls les environnements 64 bits sont actuellement pris en charge pour une installation client native.

## Exigences réseau
{: #network}

Le client Secure Gateway utilise le port de communications sortantes 443 et le port 9000 pour se connecter au registre npm et à l'environnement {{site.data.keyword.Bluemix}} :
- Port 443 pour installation npm
  - Lors de l'installation, le programme d'installation se connecte au registre npm et exécute `npm install` pour installer les dépendances requises par le client Secure Gateway. Avant l'installation, vérifiez que la machine que laquelle le client sera installé peut se connecter à un site Web de registre npm. npm est configuré pour utiliser par défaut le registre public npm d'Inc.à l'adresse https://registry.npmjs.org. <br><br>
Si votre environnement dispose d'un serveur d'entreprise npm, placez en liste blanche toutes les dépendances du client Secure Gateway sur le serveur d'entreprise npm. Pour la liste des dépendances, voir le fichier `<Installation_directory>\ibm\securegateway\client\package.json`.<br><br>

- Port 443 pour l'authentification de passerelle


  | Région  | Hôte  |
  | --  | --  |
  | Sud des Etats-Unis  | sgmanager.ng.bluemix.net  |
  | Est des Etats-Unis  | sgmanager.us-east.bluemix.net  |
  | Royaume-Uni  | sgmanager.eu-gb.bluemix.net  |
  | Allemagne  | sgmanager.eu-de.bluemix.net  |
  | Sydney  | sgmanager.au-syd.bluemix.net  |


- Port 9000

  Le noeud de la passerelle SG, indiqué dans la configuration de la passerelle, chaque passerelle ne se trouvant pas sur le même noeud, confirmez le nom d'hôte du noeud chaque vois que vous créez la passerelle


Vérifiez et modifiez si nécessaire les règles de pare-feu et de table d'IP supplémentaires
pouvant s'appliquer. Si vos administrateurs de réseau exigent des adresses IP spécifiques, [prenez contact avec le support pour demander ces adresses pour votre environnement](./securegateway_troubleshooting.html#support).


## Détermination de la configuration matérielle
{: #hardware}

Les spécifications concernant la machine qui exécute le client Secure Gateway dépendent en grande partie du trafic qui transitera via chaque connexion.  Chaque instance du client est un processus individuel qui fournit jusqu'à 250 connexions simultanées.  Pour estimer une moyenne maximale que devrait prendre en charge la machine, calculez la taille moyenne d'une transaction sur le client (à la fois la demande et la réponse), puis mettez-la à l'échelle pour 250 transactions simultanées.  Ce nombre étant une combinaison des tailles des demandes et des réponses, le client ne doit pas dépasser cette empreinte de mémoire.  Pour des estimations plus précises, utilisez un mélange de tailles de demandes et de tailles de réponses pour mieux simuler un scénario de transaction dans le monde réel.

Pour exécuter une seule instance du client Secure Gateway, un minimum de 2 coeurs et de 4 Go de mémoire sont recommandés.

Pour des scénarios à haute disponibilité exécutant plusieurs instances du client sur la même machine, un minimum de 4 coeurs et de 8 Go de mémoire sont recommandés.
