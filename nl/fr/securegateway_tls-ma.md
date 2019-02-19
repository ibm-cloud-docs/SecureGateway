---

copyright:
  years: 2015, 2017
lastupdated: "2017-04-10"

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Authentification mutuelle TLS de Node.js
{: #nodejs-tls-ma}

Cette exemple montre comment configurer l'authentification mutuelle des deux côtés d'une destination sur site : authentification des utilisateurs et authentification des ressources.

Je sais que la ressource à laquelle je veux me connecter sera hébergée sur la même machine que le client Secure Gateway, qui sera à l'écoute sur le port 8999, et qu'une authentification mutuelle est nécessaire pour autoriser une connexion.  Avec ces informations, je peux commencer à créer ma destination.

![Authentification mutuelle TLS initiale](./images/tlsMA.png?raw=true "Authentification mutuelle TLS initiale")

## Authentification des utilisateurs
Etant donné que je crée une toute nouvelle application, je ne dispose pas de paires certificat-clé préexistantes que mon application peut utiliser. Les serveurs Secure Gateway doivent donc générer une paire automatiquement pour moi.  Pour ce faire, il me suffit de laisser vide la zone de téléchargement de l'authentification des utilisateurs.  Mais si je disposais déjà d'une paire que mon application pourrait utiliser, je téléchargerais mon certificat dans cette zone.

## Authentification des ressources

### Authentification sur site
Pour que le client Secure Gateway puisse authentifier la ressource à laquelle il se connecte, je dois fournir au client le certificat (ou la chaîne de certificats) que la ressource présentera.  Etant donné que ma ressource n'a pas de chaîne de certificats complète, il me suffit de télécharger son certificat dans la zone Authentification sur site.  Cette zone accepte jusqu'à 6 fichiers de certificat distincts (.pem, .cer, .der, .crt).

### Clé et certificat client
Si je dois spécifier comment le client Secure Gateway s'identifiera auprès de ma ressource, je peux télécharger ici un certificat et une clé que le client utilisera.  Etant donné que le client et la ressource s'exécutent sur la même machine, je peux laisser cette zone vide et laisser les serveurs Secure Gateway générer une paire automatiquement pour moi.  Si ma ressource se trouve sur un hôte distinct, je dois [générer une paire certificat-clé à télécharger](/docs/services/SecureGateway/securegateway_keygen.html).

![Authentification mutuelle TLS locale](./images/localTLSma.png?raw=true "Authentification mutuelle TLS locale")

Lorsque je crée cette destination, les serveurs Secure Gateway génèrent automatiquement une paire certificat-clé à l'usage de mon application ainsi qu'une paire que le client Secure Gateway utilisera lors de la connexion à ma ressource locale.  La destination disposera également du certificat de ma ressource locale à fournir au client Secure Gateway pour une utilisation dans l'autorité de certification lors de la connexion.  Après la création, je peux éditer ma destination afin d'afficher les informations suivantes :

![Détails de l'authentification mutuelle TLS locale](./images/editLocalTLSma.png?raw=true "Détails de l'authentification mutuelle TLS locale")

## Téléchargement des fichiers de sécurité
Le panneau Informations sur la destination de ma destination propose un lien permettant de télécharger un fichier .zip qui contient tous les certificats et clés associés à ma destination:

![Panneau Informations sur la destination de l'authentification mutuelle](./images/infoPanelMA.png?raw=true "Panneau Informations sur la destination de l'authentification mutuelle")

Une fois le fichier .zip téléchargé et les fichiers qu'il contient extraits, je dispose des éléments suivants :

Nom du fichier | Objet
-- | --
secureGatewayCert.pem | Certificat principal du serveur Secure Gateway
DigiCertCA2.pem | Certificat intermédiaire du serveur Secure Gateway
DigiCertTrustedRoot.pem | Certificat racine du serveur Secure Gateway
Nn5TJ34LyVQ_i2NOT_cert.pem | Certificat généré automatiquement que doit utiliser mon application lors de la connexion au port et à l'hôte de cloud
Nn5TJ34LyVQ_i2NOT_key.pem | Clé générée automatiquement que doit utiliser mon application lors de la connexion au port et à l'hôte de cloud
Nn5TJ34LyVQ_i2NOT_destCert.pem | Certificat généré automatiquement que doit utiliser le client SG lors de la connexion à la destination
Nn5TJ34LyVQ_i2NOT_destKey.pem | Clé générée automatiquement que doit utiliser le client SG lors de la connexion à la destination
Nn5TJ34LyVQ_clientCert.pem | Certificat généré automatiquement depuis votre passerelle que le client SG doit utiliser lorsqu'il ne disposer pas d'un fichier `_destCert` ou `_destKey`
localServerCert.pem | Certificat de ma ressource locale à utiliser dans l'autorité de certification du client Secure Gateway

## Création de l'application
Maintenant que je dispose d'un port et d'un hôte de cloud pour ma destination ainsi que des divers certificats et clés, je peux commencer à rédiger mon application pour la connexion à Secure Gateway.  Il s'agit d'une simple application Node.js qui se connectera à mon serveur TLS local, recevra une petite quantité de données, puis fermera la connexion.

```javascript
let fs = require('fs');
let tls = require('tls');

let client_opts = {
    cert : fs.readFileSync('Nn5TJ34LyVQ_i2NOT_cert.pem'),
    key : fs.readFileSync('Nn5TJ34LyVQ_i2NOT_key.pem'),
    host : 'cap-sg-prd-1.integration.ibmcloud.com',
    port : 17996,
    rejectUnauthorized : true
}

let so = tls.connect(client_opts, function() {
    console.log('Client connected, authorized:', so.authorized);
    if (!so.authorized) console.log('Client authorization error:', so.authorizationError)
});

so.on('data', function(data) {
    console.log('Client data:', data.toString())
});

so.on('error', function(err) {
    console.log('Client error',err);
});

so.on('close', function() {
    console.log('Client connection closing');
});
```
{: codeblock}

## Création du serveur TLS
Je dispose de mon serveur TLS local qui est une simple application Node.js qui sera à l'écoute des connexions, enverra un message de connexion réussie, puis fermera cette connexion.

```javascript
let fs = require('fs');
let tls = require('tls');

let server_opts = {
    cert : fs.readFileSync('localServerCert.pem'),
    key : fs.readFileSync('localServerKey.pem'),
    // pfx : fs.readFileSync(''),
    // passphrase : '',
    ca : [fs.readFileSync('Nn5TJ34LyVQ_i2NOT_destCert.pem')],
    rejectUnauthorized : true,
    requestCert : true
}

// Create the local TLS Server with the settings defined above
let server = tls.createServer(server_opts, function(socket) {
    socket.write('Successful connection message from the server');
    socket.end();
});

// Set up the various event listeners for the server and log their output
server.on('error', function(err) {
    console.log('Server error', err);
});

server.on('close', function() {
    console.log('Server closing');
});

server.on('tlsClientError', function(err) {
    console.log('Server tlsClientError', err);
});

server.listen(8999);
```
{: codeblock}

## Mise à jour de la liste de contrôle d'accès du client
Avant de tester mon application, je dois m'assurer que la liste de contrôle d'accès est correctement configurée sur mon client.  J'ai ajouté `localhost:8999` à ma liste de contrôle d'accès :

```
--------------------------------------------------------------------
           -- Secure Gateway Client Access Control List --

 Connections to unlisted rules are currently: denied

 hostname:port                path           value
 localhost:8999                            Allow
--------------------------------------------------------------------
```
{: screen}

## Test de la connexion
Maintenant que mon application, mon serveur local et mon client ont été configurés, j'obtiens la sortie suivante de mon application et de mon client :

### Application

```
Client connected, authorized: true
Client data: Successful connection message from the server
Client connection closing
```
{: screen}

### Client Secure Gateway

```
[2017-04-07 12:22:42.363] [INFO] (Client ID Nn5TJ34LyVQ_qCB) Connection #1 is being established to localhost:8999
[2017-04-07 12:22:42.381] [INFO] (Client ID Nn5TJ34LyVQ_qCB) Connection #1 to localhost:8999 was closed
```
{: screen}

## Bravo !
Nous avons correctement configuré l'authentification mutuelle pour l'authentification des utilisateurs comme pour l'authentification des ressources.
