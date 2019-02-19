---

copyright:
  years: 2015, 2017
lastupdated: "2017-04-10"

---

# A propos de {{site.data.keyword.SecureGateway}}
{: #about-sg}

## Comment la sécurité est-elle assurée ?
{{site.data.keyword.SecureGatewayfull}} gère une unique connexion chiffrée persistante (TLS v1.2) entre le client {{site.data.keyword.SecureGateway}} (dans le réseau sur site) et les serveurs {{site.data.keyword.SecureGateway}}.  Cette connexion bidirectionnelle vous permet de transmettre des données en toute sécurité entre vos ressources de cloud et vos ressources sur site.  Pour les connexions sur l'une ou l'autre extrémité (entre une ressource de cloud et des serveurs SG et entre des ressources sur site et un client SG), l'utilisateur définit le protocole (TCP, TLS, HTTP, HTTPS, UDP) et des mesures de sécurité (authentification mutuelle, tables d'IP) à leur appliquer.  

## Support bidirectionnel
En combinant la destination sur site classique et la nouvelle destination de cloud, nous avons développé un support bidirectionnel complet.  La nouvelle destination de cloud permet à un service ou une application sur site d'envoyer des informations ou des demandes à une application en cloud via le client {{site.data.keyword.SecureGateway}}.  Lors de la création d'une destination de cloud, celle-ci contient un `hôte de ressource` (le nom d'hôte ou l'adresse IP de l'application en cloud), un `port de ressource` (le port sur lequel l'application en cloud sera en mode écoute) et un `port client` (le port sur lequel le client {{site.data.keyword.SecureGateway}} sera en mode écoute).  Une fois cette destination établie, le service sur site peut se connecter au port client et commencer à envoyer des données qui seront transmises au serveur {{site.data.keyword.SecureGateway}} via le client, puis à l'application en cloud spécifiée.

Le port client fourni pour une destination doit être unique sur la passerelle associée.  Par exemple, une passerelle unique ne peut pas avoir deux destinations inverses à l'écoute sur le port 12000, mais deux passerelles peuvent avoir leur propre destination à l'écoute sur le port 12000.  Toutefois, dans ce dernier cas, si les deux passerelles sont connectées au même client, une seule de ces destinations peut être à l'écoute sur le port 12000.

### Destination sur le cloud
![Destination sur le cloud](./images/reverseDestination.png?raw=true "Destination sur le cloud")

### Destination sur site
![Destination sur site](./images/onPremDestination.png?raw=true "Destination sur site")

## Haute disponibilité
Pour assurer une haute disponibilité, créez plusieurs connexions client au même ID de passerelle.  Pour ce faire, connectez plusieurs clients à passerelle unique au même ID de passerelle et/ou établissez plusieurs connexions sur un même client multi-passerelles vers le même ID de passerelle.  Les connexions seront réparties de façon circulaire entre tous les clients connectés.
