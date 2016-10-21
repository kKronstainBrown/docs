﻿---

copyright :
  2016

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Développement de contrats intelligents pour l'intégration de chaîne de blocs {{site.data.keyword.iot_short_notm}}
{: #iotblockchain_link}
Dernière mise à jour : 30 août 2016
{: .last-updated}

Utilisez {{site.data.keyword.blockchainfull}} et l'environnement de développement Hyperledger pour créer et tester vos propres contrats intelligents à partir d'exemples de contrat fournis par IBM.
{:shortdesc}

Développez et déployez des contrats intelligents sous la forme d'exécutables de code en chaîne GoLang. Utilisez l'intégration de chaîne de blocs {{site.data.keyword.iot_short_notm}} pour déclencher des mises à jour de contrat et l'exécution de logique métier avec des données d'événement de terminal et écrire un nouvel état de grand livre sur la chaîne de blocs pour chaque transaction. 

Un environnement de développement d'intégration de chaîne de blocs {{site.data.keyword.iot_short_notm}} est constitué des composants suivants :
- Organisation {{site.data.keyword.Bluemix_notm}} :
 - Service {{site.data.keyword.iot_short_notm}} avec l'intégration de chaîne de blocs IoT activée 
 - Matrice {{site.data.keyword.blockchainfull_notm}}
 - Application Node-RED qui exécute un simulateur de terminal IoT
 **Remarque :** Vous pouvez également utiliser un environnement Node-RED déployé localement pour exécuter le simulateur. 
- Environnement local :
 - Environnement de développement Hyperledger pour développer et tester un code en chaîne de contrat intelligent. L'environnement comprend le langage GoLang.
 - Interface utilisateur de surveillance Blockchain
- Environnement GitHub :
 - Référentiel GitHub fourni par IBM pour les exemples de contrats intelligents
 - Référentiel GitHub pour déployer des contrats intelligents sur la matrice {{site.data.keyword.blockchainfull_notm}}

Le diagramme suivant illustre l'environnement de développement d'intégration de chaîne de blocs {{site.data.keyword.iot_short_notm}} :
![Architecture de l'intégration de chaîne de blocs IoT{{site.data.keyword.iot_short_notm}} ](images/architecture_contracts.svg "Chaîne de blocs IoT {{site.data.keyword.iot_short_notm}} integrationarchitecture")

## Avant de commencer
{: #byb}

Découvrez le produit {{site.data.keyword.blockchainfull_notm}}, de quelle manière il est lié au concept général de chaîne de blocs et ce qu'il peut faire pour vous :
- [{{site.data.keyword.blockchainfull_notm}}](http://www.ibm.com/blockchain/) sur IBM.com.
- [{{site.data.keyword.blockchainfull_notm}} DOCS](https://console.ng.bluemix.net/docs/services/blockchain/index.html) -  Getting started with {{site.data.keyword.blockchainfull_notm}} service.
- [{{site.data.keyword.blockchainfull_notm}} API](https://ibmblockchainapi.mybluemix.net/swagger/ui.html?scheme=http&host=127.0.0.1:3000&basepath=/) - Présentation de l'API {{site.data.keyword.blockchainfull_notm}} .
- [{{site.data.keyword.blockchainfull_notm}} for Developers](http://www.ibm.com/blockchain/for_developers.html) - Présentation de la façon dont la chaîne de blocs s'intègre dans votre environnement de développement, incluant des procédures pas à pas avec des démonstrations opérationnelles et du code déployable à exécuter sur {{site.data.keyword.Bluemix_notm}}.

## Exemples de contrats intelligents
{: #samples}

Un certain nombre d'exemples de contrats peuvent être téléchargés depuis le site [https://github.com/ibm-watson-iot/blockchain-samples](https://github.com/ibm-watson-iot/blockchain-samples). Vous pouvez utiliser les exemples de contrats comme base pour développer vos propres cas d'utilisation dans un code en chaîne déployable :

|Exemple de contrat |Description |
|:---|:---|
|[Basic blockchain contract](https://github.com/ibm-watson-iot/blockchain-samples/tree/master/simple_contract_hyperledger) |Contrat qui vous permet de suivre et stocker des données d'actif de terminal sur la chaîne de blocs
|[Trade Lane blockchain contract](https://github.com/ibm-watson-iot/blockchain-samples/tree/master/trade_lane_contract_hyperledger) |Version avancée du contrat de chaîne de blocs de base|


## Configuration de votre environnement {{site.data.keyword.blockchainfull_notm}}
{: #configure_environment}
Avant de commencer à déployer et tester des contrats intelligents, vous devez configurer votre environnement de chaîne de blocs. 

**Remarque : L'intégration de chaîne de blocs ** {{site.data.keyword.iot_short_notm}} prend en charge la connexion aux matrices {{site.data.keyword.blockchainfull_notm}} et aux matrices Hyperledger. Les exemples suivants sont basés sur l'utilisation d'{{site.data.keyword.blockchainfull_notm}}.

1. Créez et configurez votre matrice {{site.data.keyword.blockchainfull_notm}}.
L'intégration de chaîne de blocs {{site.data.keyword.iot_short_notm}} requiert la matrice {{site.data.keyword.blockchainfull_notm}} pour gérer le grand livre de chaîne de blocs, les contrats intelligents et l'infrastructure de chaîne de blocs générale. L'intégration de chaîne de blocs {{site.data.keyword.Bluemix_notm}} utilise {{site.data.keyword.blockchainfull_notm}} pour gérer les chaînes. Si vous avez accès à un environnement {{site.data.keyword.blockchainfull_notm}} existant, vous pouvez l'utiliser. Sinon, vous devez créer une instance d'{{site.data.keyword.blockchainfull_notm}} à partir du [catalogue](https://console.ng.bluemix.net/catalog/services/blockchain/) {{site.data.keyword.Bluemix_notm}}.

 1. A partir de votre tableau de bord de compte {{site.data.keyword.Bluemix_notm}}, cliquez sur **Utiliser des services ou des API**.
 2. Localisez la section expérimentale du catalogue de service et sélectionnez **Chaîne de blocs**.
   **Astuce :** Cliquez [ici](https://console.ng.bluemix.net/catalog/services/blockchain/) pour accéder directement à la page de service expérimental d'{{site.data.keyword.blockchainfull_notm}}. 
 3. Sur la page de service d'{{site.data.keyword.blockchainfull_notm}}, vérifiez les sélections pour Ajout de service :  
    - Espace - Si l'espace dont vous disposez est plus étendu que l'espace `dev` par défaut, vérifiez que vous déployez bien le service dans l'espace souhaité. 
    - Appli - Laissez non lié.
    - Nom de service - Modifiez éventuellement le nom du service et remplacez-le par un nom facile à mémoriser. Ce nom s'affiche dans la vignette {{site.data.keyword.blockchainfull_notm}} dans le tableau de bord {{site.data.keyword.Bluemix_notm}}. 
    - Plan sélectionné - Sélectionnez le plan gratuit. Il fournit deux homologues de validation et une autorité de certification. 
  4. Cliquez sur **Créer** pour déployer {{site.data.keyword.blockchainfull_notm}} sur {{site.data.keyword.Bluemix_notm}}.
  La chaîne de blocs est initialement déployée avec deux noeuds homologues. Vous pouvez ajouter d'autres noeuds selon vos besoins. 

4. Liez {{site.data.keyword.iot_short_notm}} à votre service {{site.data.keyword.blockchainfull_notm}}
    Pour écrire sur la chaîne de blocs depuis {{site.data.keyword.iot_short_notm}}, vous devez d'abord lier les services. 
     1. Dans {{site.data.keyword.Bluemix_notm}}, accédez au tableau de bord. 
     2. Sélectionnez l'espace dans lequel vous avez déployé {{site.data.keyword.blockchainfull_notm}}.
     3. Cliquez sur la vignette **Chaîne de blocs**. 
     4. Dans le panneau de gauche, cliquez sur **Données d'identification pour le service**. 
     5. Sélectionnez un ensemble de données d'identification pour le service ou cliquez sur **Ajouter des données d'identification** pour créer un nouvel ensemble de données d'identification pour le service et attribuez à cet ensemble un nom descriptif, par exemple, "IoT-Platform-integration."
     6. Dans les données d'identification pour le service au format JSON, notez les paramètres suivants :  
      - Informations homologue : `api_host` et `api_port`
      - Informations utilisateur de type 1 (client) : `username` et `secret`  

      Exemple de données d'identification pour le service
     ```json
    {
        "credentials": {
      "peers": [
      {
       "discovery_host": "169.44.63.203",
       "discovery_port": "32904",
       "api_host": "169.44.63.203",
       "api_port_tls": "443",
       "api_port": "80",
       "type": "peer",
       "network_id": "f621cde2-bdec-4897-b737-da4df144c41f",
       "container_id": "5750f7734fb06c64d70c443b1dfcf39a3f5de7b51b792294c05dbdbe7d8356f7",
       "id": "f621cde2-bdec-4897-b737-da4df144c41f_vp1",
       "api_url": "http://169.44.63.203:32905"
      },
       ...
      ],
      "users": [
      {
       "username": "user_type1_fa8e6ef0dc",
       "secret": "33401036a9"
      },
       ...
       ]
      }
     }
     ```  
     **Important :** L'utilisateur que vous sélectionnez ne doit pas être déjà enregistré avec un homologue autre que celui que vous avez sélectionné.
     7. Cliquez sur **Retour au tableau de bord** pour revenir à votre tableau de bord {{site.data.keyword.Bluemix_notm}}. 
     8. Sélectionnez l'espace dans lequel vous avez déployé {{site.data.keyword.iot_short_notm}}.
     9. Cliquez sur la vignette **{{site.data.keyword.iot_short_notm}}**.
     10. Cliquez sur **Lancer** pour ouvrir le tableau de bord {{site.data.keyword.iot_short_notm}}. 
     11. Dans le tableau de bord {{site.data.keyword.iot_short_notm}}, sélectionnez **Paramètres > Connexions** en cliquant sur ![Paramètres.](images/platform_settings.png "Paramètres") dans la barre latérale de menus.
     12. Dans la section Extensions, sous la chaîne de blocs, cliquez sur **Ajouter connexion de matrice**.
    Les zones de connexion de matrice s'affichent automatiquement sur la page, et remplacent la table.
    **Remarque :** L'intégration de chaîne de blocs doit être activée pour permettre l'ajout de matrices. Pour plus d'informations, voir [Chaîne de blocs](../../reference/extensions/index.html#blockchain) dans la rubrique Intégrations de service externe. 
     14. Entrez les informations suivantes pour vous connecter à la matrice :
      - Nom de matrice - Entrez un nom permettant d'identifier la matrice dans {{site.data.keyword.iot_short_notm}}.
      - Adresse homologue - Entrez l'adresse `api_host`. 
      - Numéro de port - Entrez le numéro `api_port` ou le numéro `api_port_tls`. Utilisez le port 80 si votre implémentation n'utilise pas TLS. Utilisez le port 443 si votre implémentation utilise TLS.
      - Utiliser TLS - Utilisez le protocole TLS pour chiffrer la communication entre {{site.data.keyword.iot_short_notm}} et le contrat dans la matrice. Les numéros de port par défaut sont définis par l'instance {{site.data.keyword.iot_short_notm}} déployée à laquelle vous vous connectez. 
      - ID utilisateur - Entrez la chaîne `username`. 
      - Valeur confidentielle de l'utilisateur - Entrez la chaîne `secret`. 
     15. Cliquez sur **Confirmer toutes les modifications**
  La table de matrice est renseignée avec la nouvelle connexion de matrice.   

## Création, test et déploiement de vos contrats intelligents
{: #test_contracts}

Vous pouvez à présent créer votre propre code en chaîne de contrat intelligent en GoLang, le tester dans l'environnement de bac à sable et déployer et tester votre propre matrice {{site.data.keyword.blockchainfull_notm}}. 

1. Créez un projet GitHub dans lequel stocker votre code en chaîne de contrat intelligent.
Les contrats intelligent que vous souhaitez déployer doivent figurer dans un référentiel GitHub public. Pour plus d'informations, voir https://github.com/.
2.  Configurez un développement Hyperledger local et un environnement de test.
Pour développer et tester votre propre code en chaîne avant de le déployer sur {{site.data.keyword.blockchainfull_notm}}, vous devez configurer un environnement de développement local. Cet environnement comprend le langage GoLang que vous utilisez afin d'écrire le code en chaîne pour vos contrats. 
 1. Configurez l'environnement de développement.
 L'environnement de développement inclut les outils dont vous avez besoin pour développer vos contrats intelligents à l'aide de la génération de code en chaîne en GoLang. Pour plus d'informations, voir [Setting up the development environment](https://github.com/hyperledger/fabric/blob/master/docs/dev-setup/devenv.md) dans la documentation Hyperledger. 
 2. Installez un environnement de débogage de code en chaîne.
 L'environnement de débogage fournit les outils dont vous avez besoin pour tester et déboguer vos contrats intelligents avant de les déployer sur {{site.data.keyword.blockchainfull_notm}}. Pour plus d'informations, voir [Writing, Building, and Running Chaincode in a Development Environment](https://github.com/hyperledger/fabric/blob/master/docs/Setup/Chaincode-setup.md) dans la documentation Hyperledger. 
 3. Configurez un réseau pour le développement.
 Le réseau pour le développement fournit un environnement de type production plus strict dans lequel vous pouvez réaliser des tests finaux sur vos contrats intelligents. Utilisez cet environnement pour réaliser des tests finaux sur les contrats testés et débogués avant de les déployer sur {{site.data.keyword.blockchainfull_notm}}. Pour plus d'informations, voir [Setting Up a Network](https://github.com/hyperledger/fabric/blob/master/docs/Setup/Network-setup.md) dans la documentation Hyperledger. 

3. Facultatif : Téléchargez les exemples de contrats intelligents fournis par IBM.
IBM fournit un certain nombre de contrats intelligents que vous pouvez télécharger et utiliser directement en l'état ou que vous pouvez adapter aux objectifs de votre organisation.
Pour télécharger les exemples de contrats :
 1. Accédez au référentiel d'exemples de chaîne de blocs à l'adresse https://github.com/ibm-watson-iot/blockchain-samples/
 Les dossiers basic_contract_hyperledger et trade_lane_contract_hyperledger contiennent respectivement les contrats de base et les contrats commerciaux. 
 3. Utilisez `git clone` dans le terminal pour cloner le projet https://github.com/ibm-watson-iot/blockchain-samples.
 **Astuce :** Vous pouvez également télécharger un fichier compressé du projet en cliquant sur **Download ZIP** depuis la page de projet. 

6. Créez et testez un contrat intelligent.
 L'utilisation de l'intégration de chaîne de blocs {{site.data.keyword.iot_short_notm}} vous permet de télécharger des contrats intelligents sous la forme d'exécutables de code en chaîne sur {{site.data.keyword.blockchainfull_notm}} afin d'exécuter une logique métier sur les données de terminal écrites sur la chaîne de blocs. Le code en chaîne de contrat intelligent est développé en GoLang.
 L'exemple de contrat se concentre sur l'écriture de données de terminal IoT pour des événements dans une chaîne de blocs à partager avec des tiers ou sur la création d'entrées de journal protégées contre la falsification.
2. Créez les exécutables de contrat.
  Le code de contrat doit être converti en un fichier exécutable avant de pouvoir être déployé sur la chaîne de blocs.
  **Remarque :** L'exemple de contrat (sample_contract_hyperledger) est déjà généré et peut être déployé en l'état.
  Procédez comme suit :
   1. Ouvrez une ligne de commande et accédez au dossier du contrat.
   2. Exécutez la commande `go generate`.
   Cette commande exécute les commandes ‘go generate’ contenues dans le code. Go generate est l'outil de programmation qui permet de générer le code de pré-génération. Dans les exemples de contrats fournis par IBM, l'outil de programmation go generate est utilisé pour créer le fichier schemas.go dans lequel est défini le schéma du contrat, ainsi que le fichier de contrat sample.go.
**Important :** Le fichier schemas.go est un composant essentiel de l'intégration de chaîne de blocs {{site.data.keyword.iot_short_notm}}. Le fichier permet à la plateforme de confirmer que le contrat est conforme à la spécification d'intégration et permet à la fonction de mappage de voir l'API de contrat à laquelle les événements de terminal peuvent être mappés. 
   2. Exécutez la commande `go build`.
   Cette commande crée un exécutable portant le même nom que le dossier. Le fichier est l'exécutable de contrat qui sera déployé sur la chaîne de blocs. 

6. Créez le contrat intelligent dans le bac à sable Hyperledger.
  Avant de déployer votre contrat intelligent finalisé sur {{site.data.keyword.blockchainfull_notm}}, vous pouvez tester et déboguer le code en chaîne dans le bac à sable Hyperledger que vous avez installé dans le cadre de votre environnement de développement.   

6. Déployez le code en chaîne du contrat intelligent sur {{site.data.keyword.blockchainfull_notm}}.
 Après avoir testé et vérifié votre contrat localement, vous pouvez le déployer sur votre matrice {{site.data.keyword.blockchainfull_notm}} pour le tester.
  1. Téléchargez votre contrat sur votre référentiel GitHub public.
  Par exemple, téléchargez le fichier sample.go sur :
  `http://github.com/{my organization}/{my project}/`
  2. Enregistrez le contrat avec l'homologue auquel vous vous êtes connecté précédemment.
  Utilisez un client REST, tel que CURL ou Postman, pour soumettre l'appel d'enregistrement. Pour plus d'informations sur l'appel d'enregistrement, voir la [documentation d'API POST registrar](https://ibmblockchainapi.mybluemix.net/swagger/ui.html?scheme=http&host=127.0.0.1:3000&basepath=/#!/Registrar/registerUser). Utilisez les informations suivantes lors de l'enregistrement :
  <ul>
  <li>URL: `http://api_host:api_port/registrar`
  <li>Type: POST
  <li>Header: `Content type: application/x-www-form-urlencoded`
  <li>Payload:  
  ```json
   {  
        "enrollId": "{username}",      
        "enrollSecret": "{secret}"    
   }
   ```

  </ul>
  3. Déployez le contrat sur l'homologue.
  Pour plus d'informations sur l'appel de déploiement, voir la [documentation d'API POST devops/deploy](https://ibmblockchainapi.mybluemix.net/swagger/ui.html?scheme=http&host=127.0.0.1:3000&basepath=/#!/Devops/chaincodeDeploy).  Utilisez les informations suivantes lors du déploiement :  
  <ul>
  <li>URL: `http://api_host:api_port/devops/deploy`
  <li>Type: POST
  <li>Header: `Content type: application/x-www-form-urlencoded`
  <li>Payload:  
  ```
  {
      "type": "GOLANG",   
      "chaincodeID": {  
      "path": "http://github.com/{my organization}/{my project}/sample.go",
      "name": "string"
    },
    "ctorMsg": {  
      "function": "init",  
      "args": [
        "{\"version\":\"1.0\}"}"
      ]
    },
    "secureContext": "'username'",
    "confidentialityLevel": "PUBLIC"
  }
  ```  
  </ul>  
  Votre contrat est déployé sur la matrice.   
  **Important :** Notez l'ID de contrat renvoyé, sous la forme d'une chaîne alphanumérique composée de 128 caractères. Vous avez besoin de l'ID de contrat pour mapper des terminaux au contrat.   

10. Mappez les données de terminal au nouveau contrat intelligent.
  Avant d'écrire des données de terminal sur les nouveaux contrats intelligents de chaîne de blocs, vous devez d'abord mapper les données de terminal aux contrats.   
   1. Dans {{site.data.keyword.Bluemix_notm}}, accédez au tableau de bord. 
   2. Sélectionnez l'espace dans lequel vous avez déployé {{site.data.keyword.iot_short_notm}}.
   3. Cliquez sur la vignette **{{site.data.keyword.iot_short_notm}}**.
   4. Cliquez sur **Lancer** pour ouvrir le tableau de bord {{site.data.keyword.iot_short_notm}}. 
   5. Sélectionnez **Chaîne de blocs** en cliquant sur ![Chaîne de blocs.](images/platform_blockchain.png "Chaîne de blocs") dans la barre latérale de menus.
   6. Cliquez sur **Lier le contrat**.
   6. Sélectionnez le nom de la matrice que vous avez créée précédemment.
   7. Entrez les informations suivantes :  
     - ID de contrat : Collez l'ID de contrat de 128 caractères que vous avez sauvegardé lors du déploiement du contrat. 
     - Nom de contrat : Entrez un nom permettant d'identifier le contrat dans {{site.data.keyword.iot_short_notm}}.
     - Sélectionnez le type de terminal pour lequel vous souhaitez stocker des données de terminal dans la chaîne de blocs. 
     - Sélectionnez le nom des événements que vous souhaitez stocker.
 **Astuce :** Afin de rechercher les types d'événement pour un terminal, accédez à la page **Terminaux** et cliquez sur le nom de terminal pour ouvrir la page des détails de terminal. Faites défiler l'écran vers le bas jusqu'à la section **Informations sur le capteur** pour voir la liste des événements et points de données disponibles pour le terminal. 

   11. Mappez les propriétés de terminal disponibles aux paramètres de contrat.
   **Important :** Vérifiez que le type de données pour chaque point de données que vous mappez correspond au type de données requis par le paramètre de contrat auquel vous le mappez.  Par exemple, une propriété de contrat, telle que l'ID d'actif du type chaîne, doit être mappée à une propriété du type chaîne. Les exigences liées au paramètre de contrat sont définies dans les définitions `type` du contrat go-code.
   Par exemple, le contrat de base fourni par IBM possède les exigences de paramètre de contrat suivantes :
<ul>
    <li>  AssetID - chaîne
    <li>  Location - géolocalisation
    <ul>
    <li> Latitude - flottant 64
    <li>  Longitude - flottant 64
    </ul>
    <li>  Temperature - flottant 64
    <li>  Carrier - chaîne
    </ul>  
    Pour plus d'informations sur le mappage des données de terminal aux contrats, voir [Data mapping example](https://github.com/ibm-watson-iot/blockchain-samples/wiki/Data-mapping-example) dans le wiki des exemples IoT Blockchain sur GitHub.
   12. Sur la page de synthèse, vérifiez que les informations sont correctes. 
   13. Le mappage de données de terminal au contrat s'affiche sur la page de chaîne de blocs. 

7. Testez votre contrat intelligent dans {{site.data.keyword.blockchainfull_notm}}.
Pour tester votre contrat intelligent, effectuez un test de bout en bout en créant un  terminal dans {{site.data.keyword.iot_short_notm}}, en connectant votre terminal à {{site.data.keyword.iot_short_notm}}, en configurant IoT Blockchain pour qu'il se connecte à votre matrice de chaîne de blocs et en configurant  {{site.data.keyword.iot_short_notm}} pour qu'il mappe et stocke vos messages de terminal dans la chaîne de blocs. A l'aide de la console {{site.data.keyword.blockchainfull_notm}}, vous pouvez afficher la chaîne de blocs pour voir les données de terminal dans le grand livre. Si votre contrat prend en charge la fonction readAsset(), vous pouvez utiliser l'interface utilisateur de surveillance pour afficher votre chaîne de blocs et voir les données de terminal de votre propre scénario stockées de manière indélébile dans une chaîne de blocs. 

5. Configurez l'interface utilisateur de surveillance pour qu'elle se connecte à {{site.data.keyword.blockchainfull_notm}}.
 **Astuce :** Si vous n'avez pas encore installé l'interface utilisateur de surveillance dans votre environnement local, vous pouvez l'installer dès maintenant. Suivez les instructions décrites dans le fichier Readme Monitoring UI disponible dans le répertoire GitHub [Blockchain Monitoring UI](https://github.com/ibm-watson-iot/blockchain-samples/tree/master/monitoring_ui). Accédez aux paramètres de configuration en cliquant sur le bouton **CONFIGURATION**. Utilisez les informations suivantes pour vous connecter à un contrat :
<table>
<thead>
<tr>
<th>Paramètre</th>
<th>Valeur</th>
<th>Commentaire</th>
</tr>
</thead>
<tbody>
<tr>
<td>Hôte et port d'API</td>
<td>`http://peer_URL:port`</td>
<td>Hôte et port de l'API REST {{site.data.keyword.blockchainfull_notm}} auxquels `http://` est ajouté en préfixe. Utilisez l'adresse `api_host` et le numéro `api_port`. </td>
</tr>
<tr>
<td>ID de code en chaîne</td>
<td>ID de contrat qui a été renvoyé lors de l'enregistrement du contrat. </td>
<td>L'ID de contrat est une chaîne alphanumérique de 128 caractères qui correspond à l'entrée ID de contrat.   
**Important :** Lorsque vous copiez et collez l'ID de contrat, assurez-vous qu'aucun espace n'est inclus dans l'ID. Si l'ID saisi est incorrect, les entrées de grand livre de chaîne de blocs s'affichent, mais la fonction de recherche d'actif ne fonctionne pas.
</td>
</tr>
<tr>
<td>Contexte sécurisé</td>
<td>Votre utilisateur de matrice</td>
<td>Ce paramètre est requis pour la connexion aux instances {{site.data.keyword.blockchainfull_notm}} sur {{site.data.keyword.Bluemix_notm}}. Utilisez l'entrée `secureContext`.   
**Important :** La valeur de l'entrée secureContext doit être identique à celle de l'entrée `username` que vous avez utilisée pour configurer la matrice.
</td>
</tr>
<tr>
<td>Nombre de blocs à afficher</td>
<td>Entier positif. Valeur par défaut : 10</td>
<td>Nombre de blocs de chaîne de blocs à afficher.
</td>
</tr>
</tbody>
</table>

3. Dans l'interface utilisateur de surveillance, vérifiez que votre configuration fonctionne comme prévu. Utilisez les composants de l'interface utilisateur de surveillance pour interagir avec votre contrat de chaîne de blocs :   
 - Opérations sur le code en chaîne
 Vérifiez que les opérations sur le code en chaîne propres au contrat peuvent être exécutées comme prévu. Par exemple, pour le contrat de base, vérifiez que l'exécution d'une fonction `createAsset` entraîne l'ajout d'un actif sur la chaîne de blocs. 
 - Contenu de réponse
 Vérifiez que les réponses aux demandes des homologues apparaissent comme prévu lorsque vous soumettez des demandes REST à partir de l'onglet des opérations sur le code en chaîne. 
 - Chaîne de blocs
Vérifiez que des blocs sont ajoutés à la chaîne lorsque vous injectez des données à partir d'un terminal lié ou lorsque vous utilisez le composant des opérations sur le code en chaîne.     

## Etapes suivantes
{: #next_steps}

Vous avez déployé et exploré l'exemple de contrat intelligent fourni par IBM. Toutefois, les contrats de base et commerciaux fournissent des exemples limités des nombreuses possibilités que peut offrir un code en chaîne de contrat intelligent correctement conçu. A présent, il est temps d'expérimenter et de mapper vos scénarios métier à des contrats de code en chaîne dans {{site.data.keyword.blockchainfull_notm}}. Pour ce faire, vous pouvez utiliser {{site.data.keyword.iot_short_notm}} avec l'intégration de chaîne de blocs IoT afin d'écrire des données de terminal sur le grand livre de chaîne de blocs et exécuter la logique métier stockée sous forme de contrats intelligents dans la réponse aux données.     


Faites de vos opérations de chaînage de blocs un jeu d'enfants !
