---

copyright:
  years: 2015, 2017
lastupdated: "2017-04-25"

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Gestion des certifications et des clés

## Génération d'une paire certificat-clé autosignée

Pour générer une paire certificat-clé autosignée, exécutez la commande suivante :

```
openssl req -new -newkey rsa:2048 -days 365 -nodes -x509 -keyout serverKey.pem -out serverCert.pem
```
{: pre}


## Téléchargement d'une paire certificat-clé dans le navigateur

Pour accéder à une destination qui exige l'authentification mutuelle, vous devez convertir votre paire certificat-clé en fichier PKCS#12 et le télécharger dans votre navigateur.

Pour créer le fichier PKCS#12, exécutez la commande suivante :

```
openssl pkcs12 -export -in <chemin_certificat> -inkey <chemin_clé> -name "my-sg-pair" -out <chemin_où_écrire>.p12
```
{: pre}

Dans Firefox, le fichier .p12 peut être téléchargé via Options > Préférences > Avancé > Certificats > Afficher les certificats > Vos certificats > Importer

Dans Chrome, le fichier .p12 peut être téléchargé via Paramètres > HTTPS/SSL > Gestion des certificats > Importer

Dans Internet Explorer, le fichier .p12 peut être téléchargé via Paramètres > Contenu > Certificats > Importer
