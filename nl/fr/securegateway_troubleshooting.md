---

copyright:
  years: 2015, 2018
lastupdated: "2018-08-10"

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Traitement des incidents

## Meilleures pratiques pour l'exécution du client Secure Gateway
{: #best}

- Exécutez le client Secure Gateway sur une partition de système d'exploitation qui offre une visibilité réseau des services reliés par le client lui-même. Par exemple, certains environnements de virtualisation hébergés prennent en charge plusieurs modes de connectivité du réseau, notamment la conversion NAT et
l'utilisation d'un pont. Veillez à choisir le type de connexion approprié qui permet d'accéder aux services
{{site.data.keyword.Bluemix}} depuis Internet.
- Installez Secure Gateway dans votre environnement informatique, là où les règles de sécurité de votre entreprise le permettent. En général, vous pouvez l'installer dans une zone jaune protégée ou dans une zone démilitarisée, où votre
entreprise peut appliquer les contrôles de sécurité appropriés afin de protéger les actifs sur site. Respectez toujours les règles de sécurité et les instructions de votre entreprise lorsque vous installez le client Secure Gateway.
- Avant d'installer un client dans votre environnement, vérifiez qu'Internet et vos actifs sur site sont
accessibles et que les tous les noms d'hôte peuvent être résolus par un serveur de noms de domaine (DNS).

## Etapes initiales de traitement des incidents

- Initiez la demande en échec depuis l'application demandeuse
- Consultez les journaux du client Secure Gateway
- Si aucun journal de client n'a été généré à partir de la demande, le problème se situe entre l'application demandeuse et les serveurs Secure Gateway.  Le problème peut aller de la fiabilité du réseau à des protocoles de requête non concordants, en passant par un établissement de liaison d'authentification mutuelle TLS inapproprié.
- Si le client a généré des journaux d'erreurs à partir de la requête, le problème se situe entre le client SG et la ressource sur site.  Le tableau ci-dessous répertorie les erreurs communes, les problèmes généralement à l'origine de ces erreurs et les méthodes susceptibles de les résoudre.

Erreur | Cause typique | Méthodes de traitement
--- | --- | ---
ETIMEDOUT | Le client ne parvient pas à trouver le nom d'hôte/l'adresse IP de connexion en raison de contraintes de réseau. | Essayez d'envoyer une commande ping au nom d'hôte ou à l'adresse IP de destination depuis l'hôte qui exécute le client afin de vérifier la connectivité du réseau.  Si vous exécutez la version Docker du client, établir un pont entre le conteneur où réside le système d'exploitation de l'hôte et `--net=host` peut régler le problème.
ECONNREFUSED | Le client a résolu le nom d'hôte/l'adresse IP de connexion mais ne parvient pas à lancer la procédure d'établissement de liaison de connexion. | Ce problème est généralement lié à une non-concordance de protocole entre le client SG et la ressource sur site (par exemple, le client essaie d'établir une connexion TCP à un hôte:port qui attend une connexion TLS).  Dans certains cas, une règle de pare-feu peut provoquer cette erreur au lieu de l'erreur ETIMEDOUT.
ECONNRESET | Le client a établi une connexion à la destination mais une erreur s'est produite soit pendant l'établissement de liaison (une erreur d'établissement de liaison TLS peut également engendrer différentes erreurs) ou pendant le traitement de la demande par la ressource sur site. | Vous devez consulter les journaux de la ressource sur site pour vous assurer qu'aucune erreur n'a entraîné l'interruption de la connexion.  Si vous ne détectez pas d'erreurs dans les journaux de la ressource sur site, vérifiez dans la configuration de la destination si les protocoles appropriés (et, au besoin les certificats) sont fournis au client pour la connexion.
REMOTE_RST | Une erreur s'est produite côté serveur SG. <br><br> Pour une destination sur site, il s'agit d'une erreur lors de la connexion de l'application demandeuse au serveur SG, ou d'une erreur de temporisation lors de la réception de données depuis la ressource sur site. <br><br> Pour une destination cloud, il peut s'agir de toute erreur allant d'un échec d'établissement de liaison TLS jusqu'à des erreurs au niveau de la ressource de cloud. | Pour une destination sur site, assurez-vous que l'application demandeuse utilise les protocoles appropriés pour établir la connexion au serveur SG ; si l'erreur se produit lors de la réception de données depuis la ressource sur site, essayez d'augmenter/de désactiver le délai d'attente. <br><br> Pour une destination cloud, consultez les journaux de la ressource de cloud pour vous assurer qu'aucune erreur n'a entraîné l'interruption de la connexion.  Si vous ne détectez pas d'erreurs dans les journaux de la ressource de cloud, vérifiez dans la configuration de la destination si les protocoles appropriés (et, au besoin les certificats) sont fournis au client pour la connexion.

Un grand nombre d'applications "se bloquent" après qu'une erreur ECONNRESET s'est produite à l'autre extrémité du tunnel. Ce comportement est normal. Secure
Gateway ne peut pas réexécuter le paquet RST à l'autre extrémité du tunnel car les paquets TCP ont déjà bénéficié d'un accusé de réception de ce côté
du tunnel. Les délais d'attente au niveau de l'application, l'application ne recevant jamais de réponse d'accusé de réception, sont la seule méthode pour mettre fin au blocage.

## Configuration de votre client Docker de sorte qu'il redémarre lorsque votre serveur redémarre
{: #docker}

### Symptôme
Lorsque vous redémarrez le serveur sur lequel s'exécute votre client Secure Gateway, vous devez redémarrer manuellement le client Docker de Secure Gateway. Comment configurer le client pour qu'il démarre automatiquement après le
redémarrage du système ?

### Procédure de résolution du problème

- Sur les systèmes Linux et UNIX :
- Intégrez la commande Docker dans un script pouvant être appelé suite à l'exécution d'un travail CRON.
- Si vous utilisez un poste de travail Linux, vous pouvez créer une configuration upstart pour garantir que le client est démarré à chaque fois que le serveur est redémarré. Pour plus d'informations, voir Automatically Start Containers sur le site Web Docker.
- Sur les systèmes Windows, émettez la commande suivante pour démarrer le client :

```
for /L %i in (0,0,0) do docker run -it ibmcom/secure-gateway-client <id_passerelle>
```
{: pre}

## Message d'erreur de connexion : host is not in cert's CN
{: #not-in-cn}

### Symptôme
Vous tentez d'implémenter une connexion TLS côté client sur site à l'aide du client Secure Gateway et recevez le message d'erreur suivant :

```
[ERROR] Connection #<ID connexion> had error: Host: <nom d'hôte>. is not cert's CN: <mon_nom_usuel>

Où :
    - ID connexion est un numéro de connexion affecté par le client.
    - nom d'hôte est le nom d'hôte pour la connexion.
    - mon_nom_usuel est le nom de domaine complet ou le nom usuel qui est utilisé dans le certificat.
```
{: screen}

### Cause
Les noms usuels, par exemple le nom de domaine complet du serveur ou votre nom,
pour votre application sur site et le certificat que vous avez téléchargé dans
{{site.data.keyword.Bluemix_notm}} pour cette destination ne correspondent pas.

- Vérifiez les points suivants :
- Vous avez généré le certificat correctement et vous avez utilisé le nom de domaine complet du serveur ou le nom d'hôte correct.
- Vous avez téléchargé le certificat correct dans votre destination {{site.data.keyword.Bluemix_notm}} pour ce
client.

### Procédure de résolution du problème

 1. Dans l'interface utilisateur {{site.data.keyword.Bluemix_notm}}, accédez au tableau de bord Secure Gateway.
 2. Sélectionnez votre destination, puis cliquez sur l'icône Editer.
 3. Cliquez sur Télécharger le certificat.
 4. Téléchargez le fichier certificat PEM à utiliser pour la connexion au système sur site.

## IP is not in the cert's list
{: #san}

### Symptôme
Le nom usuel dans le certificat présenté est l'adresse IP de la passerelle, mais le certificat n'a pas de réseau SAN correspondant à l'adresse IP et le client n'arrive pas à se connecter.  

En raison de problèmes de résolution des noms d'hôte, nous utilisons l'adresse IP dans notre destination.  Le nom usuel dans le certificat présenté est l'adresse IP de la passerelle, mais le certificat n'a pas de réseau SAN correspondant à l'adresse IP et le client n'arrive pas à se connecter.

Vous avez créé une destination en utilisant TLS, mais au lieu d'utiliser le nom d'hôte de la destination, vous avez utilisé son adresse IP.  Lors de la connexion au client l'erreur suivante est émise.

```
[2015-10-15 13:00:04.866] [INFO] Connection #0 is being established to 10.3.20.31:443
[2015-10-15 13:00:05.426] [ERROR] Connection #0 to destination 10.3.20.31:443 had error: IP: 10.3.20.31 is not in the cert's list:
[2015-10-15 13:00:05.427] [INFO] Connection #0 to 10.3.20.31:443 was closed
```
{: screen}

### Cause
Le code de vérification SSL dans le client de la passerelle traite la destination différemment car il utilise une adresse IP plutôt qu'un nom d'hôte.  Au lieu d'établir une correspondance avec le nom usuel du certificat, il recherche une correspondance avec l'adresse IP dans le réseau SAN du certificat.  Comme le certificat ne comporte pas de réseau SAN, le code de vérification considère la connexion comme incorrecte et l'établissement de liaison SSL échoue.

### Procédure de résolution du problème
Si vous examinez le message d'erreur, vous constatez qu'il ne mentionne pas le nom usuel ou CN, (exemple, [ERROR] Connection ## had error: Host: . is not cert&apos;s CN: ), mais la liste des certificats, ce qui laisse supposer que vous avez généré votre certificat autosigné de manière incorrecte. Le problème est lié au fait que le certificat a été généré à l'aide d'un nom de domaine ou d'un nom usuel avec une adresse IP. Cela ne fonctionnera pas car les adresses IP ne sont prises en charge que si vous utilisez un réseau SAN.

Génération d'un certificat avec une adresse IP comme nom usuel avec openssl :

1. Créez un fichier de configuration openssl. Vous pouvez le copier depuis /usr/lib/ssl/openssl.cnf
2. Ajoutez une section alternate_names comme indiqué ci-dessous :

   ```
   [ alternate_names ]

   IP.1 = &lt;IP de mon application&gt;
   ```
   {: codeblock}

3. Dans la section [ v3_ca ], ajoutez la ligne suivante :

    ```
    subjectAltName = @alternate_names
    ```
    {: pre}

4. Sous la section CA_default, supprimez la mise en commentaire de copy_extensions (option de copie d'extension : à utiliser avec précaution):

    ```
    copy_extensions = copy
    ```
    {: pre}

5. Générez une clé privée

    ```
    openssl genrsa -out private.key 3072
    ```
    {: pre}

6. Générez le certificat avec des options d'organisation

    ```
    openssl req -new -x509 -key private.key -sha256 -out certificate.pem -days 730 -config
    ```
    {: pre}

7. Combinez les fichiers

    ```
    cat private.key certificate.pem > SAN.pem
    ```
    {: pre}

8. Chargez le fichier SAN.pem dans votre destination en tant que certificat client TLS.
9. Chargez le fichier SAN.pem dans votre application sur site et redémarrez.

10. Votre application peut être configurée pour TCP, HTTP ou HTTPS et votre application côté cloud doit désormais pouvoir se connecter à votre application sur site.

Si vous rencontrez un problème UNABLE_TO_VERIFY_LEAF_SIGNATURE, vérifiez votre fichier openssl.conf, et modifiez :

    ```
    keyUsage = digitalSignature, keyEncipherment
    ```
    {: pre}

avec la valeur par défaut :

    ```
    keyUsage = nonRepudiation, digitalSignature, keyEncipherment
    ```
    {: pre}

## Message d'erreur de connexion : DEPTH_ZERO_SELF_SIGNED_CERT
{: #depth-zero}

### Symptôme
Vous tentez d'implémenter une connexion TLS côté client sur site à l'aide du client Secure Gateway et recevez le message d'erreur suivant :

```
[ERROR] Connection #<ID connexion> had error: DEPTH_ZERO_SELF_SIGNED_CERT Où :

    ID connexion est un numéro de connexion affecté par le client.
```
{: screen}

### Cause
Un certificat côté client manque dans la destination que vous avez définie.

### Procédure de résolution du problème
 1. Dans l'interface utilisateur {{site.data.keyword.Bluemix_notm}}, accédez au tableau de bord Secure Gateway.
 2. Sélectionnez votre destination, puis cliquez sur l'icône Editer.
 3. Cliquez sur Télécharger le certificat.
 4. Téléchargez le fichier certificat PEM à utiliser pour la connexion au système sur site.


## Comment puis-je charger un fichier ACL dans le client Docker de manière interactive ?
{: #docker-acl}

### Symptôme
Etant donné que Docker est un conteneur ou un environnement virtualisé, il ne dispose pas d'un accès direct à votre système de fichiers tant que le conteneur n'est pas démarré.  Il ne peut donc pas lire le système de fichier de vos machines hôte tant qu'il n'est pas effectivement démarré et en cours d'exécution.

### Procédure de résolution du problème
Votre intervention :

- Créez un fichier Dockerfile pour inclure le fichier aclfile.txt

```
FROM ibmcom/secure-gateway-client
ADD aclfile.txt /tmp/aclfile.txt
```
{: codeblock}

- Générez une nouvelle image Docker

```
docker build -t ads-secure-gateway-client .
```
{: pre}

- Exécutez la nouvelle image Docker (vous devez spécifier les options -t et -i, sinon vous obtenez une erreur signalant que le fichier est introuvable ou ENOENT):

```
docker run -t -i ads-secure-gateway-client1 --F /tmp/aclfile.txt
```
{: pre}

- Accédez à la sortie suivante :

```
[2015-09-30 16:50:32.084] [INFO] The current access control list is being reset and replaced by the user provided file: /tmp/aclfile.txt
[2015-09-30 16:50:32.086] [INFO] The ACL file process accepts acl allow :8000
[2015-09-30 16:50:32.087] [INFO] The ACL file process accepts acl deny local
```
{: screen}

## Aide et assistance supplémentaires
{: #support}

Si vous avez des questions techniques sur le développement ou le déploiement d'une application avec Secure Gateway, soumettez votre question sur le site [Stack Overflow ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](http://stackoverflow.com/search?q=securegateway+ibm-bluemix).  Marquez votre question avec les étiquettes "ibm-bluemix" et "secure-gateway" pour que les équipes de développement {{site.data.keyword.Bluemix_notm}} la repère plus facilement.

Si vous avez une question concernant le service ou les instructions de démarrage, accédez au forum [IBM developerWorks dW Answers ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://developer.ibm.com/answers/topics/securegateway/?smartspace=bluemix) et utilisez les étiquettes "bluemix" et "Secure Gateway".

Pour plus d'informations sur l'utilisation de ces forums, cliquez sur la [page Obtenir de l'aide ici](https://console.ng.bluemix.net/docs/support/index.html#getting-help).

Pour plus d'informations sur l'ouverture d'un ticket de demande de service IBM, sur les niveaux de support disponibles ou les niveaux de gravité des tickets, voir la rubrique décrivant [comment contacter le support](https://console.ng.bluemix.net/docs/support/index.html#contacting-support).

Lors de la soumission d'un ticket, fournissez le maximum des informations suivantes :

- Sous quel système d'exploitation le client s'exécute
- Quelle version du client est utilisée (la commande 'C' sur le client permet d'obtenir cette information)
- S'il s'agit d'un problème d'interface utilisateur, collez ou mettez en pièce jointe tous les journaux et captures d'écran associés de la console du navigateur
- Collez ou mettez en pièce jointe tous les journaux associés à de l'application demandeuse
- Collez ou mettez en pièce jointe tous les journaux associés au client Secure Gateway
- Donnez des détails concernant la destination utilisée (une capture d'écran ou renseignez les zones suivantes) :
   - ID de la destination
   - Protocole
   - Authentification côté destination
   - Certificats chargés (uniquement leurs noms et la zone dans laquelle ils ont été chargés)
