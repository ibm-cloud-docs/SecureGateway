---

copyright:
  years: 2015, 2019
lastupdated: "2019-03-07"

---

# Exigences pour exécuter le client
{: #client-requirements}

## Configuration système requise
{: #system-requirements}

Le client Secure Gateway est pris en charge dans les environnements suivants :

| Nom | Versions          |
| ------------- | ----------- |
| Windows Desktop | 7, 8.1, 10 et versions ultérieures |
| Windows Server | 2012 R2 et versions ultérieures |
| Red Hat Linux | 7.3 et versions ultérieures |
| CentOS | 7.3 et versions ultérieures |
| SuSE Linux | 12 et versions ultérieures |
| Ubuntu Linux | 14.04 (LTS) et versions ultérieures |
| Power Machine | architecture ppc64el |
| Ubuntu Z-Linux | - |
| AIX | 7.1 TL4 et versions ultérieures |
| Mac OS X | 10.10 (Yosemite) et versions ultérieures |
| Docker | 1.7.0 et versions ultérieures, tous les systèmes d'exploitation pris en charge |

<b>Remarque :</b> seuls les environnements 64 bits sont actuellement pris en charge pour une installation client native.

## Exigences réseau
{: #network-requirements}

Le client Secure Gateway utilise le port de communications sortantes 443 et le port 9000 pour se connecter au registre npm et à l'environnement {{site.data.keyword.Bluemix}} :
- Port `443` pour l'installation npm
  - Lors de l'installation, le programme d'installation se connecte au registre npm et exécute `npm install` pour installer les dépendances requises par le client Secure Gateway. Avant l'installation, vérifiez que la machine que laquelle le client sera installé peut se connecter à un site Web de registre npm. npm est configuré pour utiliser par défaut le registre public npm d'Inc. à l'adresse `https://registry.npmjs.org`. <br><br>
Si votre environnement dispose d'un serveur d'entreprise npm, placez en liste blanche toutes les dépendances du client Secure Gateway sur le serveur d'entreprise npm. Pour obtenir la liste des dépendances, voir le fichier `<Installation_directory>\ibm\securegateway\client\package.json`. <br><br>

- Port `443` pour l'authentification de passerelle
  - Pour le client SG `v180fp9 et versions antérieures`


  | Région  | Hôte  |
  | --  | --  |
  | Sud des Etats-Unis  | sgmanager.ng.bluemix.net  |
  | Est des Etats-Unis  | sgmanager.us-east.bluemix.net  |
  | Royaume-Uni  | sgmanager.eu-gb.bluemix.net  |
  | Allemagne  | sgmanager.eu-de.bluemix.net  |
  | Sydney  | sgmanager.au-syd.bluemix.net  |

  - Pour le client SG `v181 et versions ultérieures`
  
  
  | Région  | Hôte  |
  | --  | --  |
  | Sud des Etats-Unis  | sgmanager.us-south.securegateway.cloud.ibm.com  |
  | Est des Etats-Unis  | sgmanager.us-east.securegateway.cloud.ibm.com  |
  | Royaume-Uni  | sgmanager.eu-gb.securegateway.cloud.ibm.com  |
  | Allemagne  | sgmanager.eu-de.securegateway.cloud.ibm.com  |
  | Sydney  | sgmanager.au-syd.securegateway.cloud.ibm.com  |

- Port `9000`

  Il s'agit du noeud de la passerelle SG, accessible depuis la configuration de la passerelle. Chaque passerelle ne se trouvant pas sur le même noeud, confirmez le nom d'hôte du noeud chaque fois que vous créez la passerelle


Vérifiez et modifiez si nécessaire les règles de pare-feu et de table d'IP supplémentaires
pouvant s'appliquer. Toutefois, il n'est pas conseillé de configurer des règles par IP. Les règles devraient être définies sur le nom d'hôte et le port, car les adresses IP de l'authentification de passerelle et la passerelle SG sont contrôlées par Bluemix et sont sujettes à modification. Si vos administrateurs de réseau exigent les adresses IP actuelles pour un nom d'hôte spécifique, [prenez contact avec le support pour demander ces adresses pour votre environnement](/docs/services/SecureGateway?topic=securegateway-troubleshooting#getting-help-and-support).


## Détermination de la configuration matérielle
{: #hardware-requirements}

Les spécifications concernant la machine qui exécute le client Secure Gateway dépendent en grande partie du trafic qui transitera via chaque connexion.  Chaque instance du client est un processus individuel qui fournit jusqu'à 250 connexions simultanées.  Pour estimer une moyenne maximale que devrait prendre en charge la machine, calculez la taille moyenne d'une transaction sur le client (à la fois la demande et la réponse), puis mettez-la à l'échelle pour 250 transactions simultanées.  Ce nombre étant une combinaison des tailles des demandes et des réponses, le client ne doit pas dépasser cette empreinte de mémoire.  Pour des estimations plus précises, utilisez un mélange de tailles de demandes et de tailles de réponses pour mieux simuler un scénario de transaction dans le monde réel.

Pour exécuter une seule instance du client Secure Gateway, un minimum de 2 coeurs et de 4 Go de mémoire sont recommandés.

Pour des scénarios à haute disponibilité exécutant plusieurs instances du client sur la même machine, un minimum de 4 coeurs et de 8 Go de mémoire sont recommandés.
