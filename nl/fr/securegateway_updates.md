---

copyright:
  years: 2015, 2017
lastupdated: "2017-04-25"

---

# Journal des modifications de {{site.data.keyword.SecureGateway}}
{: #secure-gateway-change-log}

## Version 1.7.0 Groupe de correctifs 1

### Correctifs

- Correction de l'erreur `EADDRINUSE` liée à des programmes d'écoute non correctement fermés lors d'une suppression de destination.
- Résolution de l'impossibilité de supprimer des instances de service associées à une application.

## Version 1.7.0

### Fonctionnalités

- Introduit de nouveaux plans de service Essentials, Professional et Enterprise.
- Le client Secure Gateway prend désormais en charge un proxy SOCKS entre lui-même et la destination sur site.

## Version 1.6.0 Groupe de correctifs 1

### Correctifs

- Résolution du problème de génération de processus orphelins dans la grappe de clients connectés lors de la déconnexion de plusieurs clients.
- Les connexions orphelines résultant d'une mise en pause incorrecte des programmes d'écoute sont désormais supprimées.

## Version 1.6.0

### Fonctionnalités

- La liste de contrôle d'accès prend désormais en charge un routage de chemin spécifique pour les demandes HTTP/S.
- Les destinations prennent maintenant en charge les indicateurs de noms de serveur pour les connexions TLS.

## Version 1.5.1

### Fonctionnalités

- Un assistant de destination a été ajouté pour la création de destination.
- Les passerelles prennent en charge le téléchargement d'une paire certificat-clé personnalisée.
- Les services sont désormais limités à 50 passerelles et 50 destinations par passerelle.
- Les destinations/passerelles/services peuvent désormais être exportés/importés.
- Des tests du temps d'attente entre le client et le serveur peuvent être effectués à partir de l'interface utilisateur de Secure Gateway.

## Version 1.5.0 Groupe de correctifs 1

### Correctifs

- Nettoyage des processus de tunnel orphelins à la déconnexion du réseau.
- Résolution de l'allocation de ports en double provoquée par un échec de suppression d'une destination.
- Résolution des problèmes HTTP/S avec codage non UTF8.
- Résolution des gestionnaires IPC manquants dans les processus de remplacement.

## v1.5.0

### Fonctionnalités

- Le client dispose désormais d'une interface utilisateur interactive locale.
- Les connexions UDP sont désormais prises en charge.
- L'empreinte mémoire du client a été considérablement réduite.
- Le mode client unique n'est plus pris en charge ; le mode multi-client est celui par défaut et transparent pour un client à passerelle unique.
- La liste de contrôle d'accès est synchronisée entre les clients connectés à la même passerelle.

## Version 1.4.2

### Fonctionnalités

- Un ID est désormais affecté aux clients lorsqu'ils se connectent au serveur SG.
- Les clients peuvent désormais être arrêtés à distance via une API ou via l'interface utilisateur de Secure Gateway.
- Vous pouvez vérifier le statut de connexion d'un client à l'aide d'une API.
- La fonctionnalité TLS côté destination prend désormais en charge l'authentification mutuelle.
- Les destinations sur cloud prennent désormais en charge les protocoles d'authentification mutuelle.
