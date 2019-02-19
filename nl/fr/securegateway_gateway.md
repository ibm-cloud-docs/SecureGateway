---

copyright:
  years: 2015, 2017
lastupdated: "2017-04-10"

---

# Ajout d'une passerelle
{: #add-sg-gw}

Une passerelle peut être considérée comme un moyen d'identifier un réseau ou un environnement particulier.  C'est l'élément utilisé par un client Secure Gateway pour établir la connectivité aux serveurs Secure Gateway et qui peut contenir plusieurs destinations ou définitions de ressource.

![Tableau de bord Secure Gateway](./images/newDashboard.png?raw=true "Tableau de bord Secure Gateway")

Dans le tableau de bord Secure Gateway, cliquez sur le bouton Ajouter une passerelle pour ouvrir le panneau de même intitulé.

![Ajouter une passerelle](./images/addGateway.png?raw=true "Ajouter une passerelle")

La seule entrée obligatoire sur ce panneau est le nom de votre passerelle.  Les cases `Exiger un jeton de sécurité` et `Expiration du jeton` sont cochées par défaut.

Si vous exigez un jeton de sécurité pour la connexion des clients, chaque fois que vous démarrez un client Secure Gateway, vous devrez fournir à la fois l'ID de passerelle et le jeton de sécurité.  Si vous ne cochez pas la case `Exiger un jeton de sécurité`, il vous suffira de fournir l'ID de passerelle du client à connecter.

La date d'expiration du jeton de sécurité est fixée par défaut à 90 jours après sa date de création.  Pour modifier la date d'expiration, laissez la case `Expiration du jeton` cochée et indiquez dans la zone de texte le nombre de jours qui vous convient avant expiration du jeton (1 jour minimum, 365 jours maximum).  Pour créer un jeton sans date d'expiration, désactivez la case `Expiration du jeton`.  

Pour finaliser la création de votre passerelle, cliquez sur Ajouter une passerelle.  Une fois la passerelle créée, la page naviguera automatiquement dans votre nouvelle passerelle.

![Nouvelle passerelle](./images/newGateway.png?raw=true "Nouvelle passerelle")

Maintenant que votre nouvelle passerelle a été créée, vous pouvez [connecter votre premier client](/docs/services/SecureGateway/securegateway_client.html).
