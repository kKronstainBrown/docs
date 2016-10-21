---

Copyright :
  Années : 2015, 2016

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen:.screen}
{:codeblock:.codeblock}
{:pre: .pre}

# Connexions d'application, de terminal et de passerelle à {{site.data.keyword.iot_short_notm}}
{: #connect_devices_apps_gw}
Dernière mise à jour : 8 septembre 2016
{: .last-updated}

Les applications, les terminaux et les passerelles peuvent se connecter à {{site.data.keyword.iot_full}} via le protocole MQTT. Les terminaux peuvent également se connecter à et publier des événements sur {{site.data.keyword.iot_short_notm}} via l'API HTTP.
{: shortdesc}


## URL de connexion client
{: #client_connect_url}

Pour connecter des clients de terminal, d'application et de passerelle à votre instance {{site.data.keyword.iot_short_notm}}, utilisez les URL de connexion suivantes :

### Adresse de messagerie

<pre class="pre"><var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com</pre>
{: codeblock}

### URL de connexion de l'API REST HTTP

<pre class="pre">https://<var class="keyword varname">orgId</var>.internetofthings.ibmcloud.com/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/events/<var class="keyword varname">eventId</var></pre>
{: codeblock}

**Remarques**
- *orgId* est l'ID d'organisation unique ayant été généré lorsque vous avez enregistré l'instance de service. 
- Si vous connectez un terminal ou une application au service Quickstart, spécifiez 'quickstart' comme valeur *orgId*. 

## Sécurité du port
{: #client_port_security}

Assurez-vous que les ports requis sont ouverts et activés pour les communications.

|Type de connexion |Numéro de port|
|:---|:---|
|Non sécurisée|1883|
|Sécurisée|8883|
|Sécurisée|443|

Les clients MQTT se connectent en utilisant des données d'identification appropriées, telles que des jetons d'authentification pour les terminaux et des clés et des jetons d'API pour les applications. Etant donné que la messagerie MQTT via le port non sécurisé 1883 envoie ces données d'identification sous forme de texte brut, utilisez toujours les alternatives sécurisées 8883 ou 443 à la place. Les ports sécurisés forcent le chiffrement des données d'identification TLS. N'oubliez pas que vous devez activer TLS dans l'application en utilisant la méthode tls_set() dans la bibliothèque Python MQTT. A défaut, les données seront peut-être envoyées de manière non sécurisée. 

Lorsque vous utilisez la messagerie MQTT sécurisée sur les ports 8883 ou 443, les bibliothèques client plus récentes font automatiquement confiance au certificat qui est présenté par {{site.data.keyword.iot_short_notm}}. Si ce n'est pas le cas pour votre environnement client, vous pouvez télécharger et utiliser la chaîne de certificats complète à partir de [messaging.pem](https://github.com/ibm-messaging/iot-python/blob/master/src/ibmiotf/messaging.pem).


## Exigences TLS
{: #tls_requirements}
Certaines bibliothèques client TLS (Transport Layer Security) ne prennent pas en charge les domaines qui incluent un caractère générique. Si vous ne pouvez pas changer de bibliothèques, désactivez la vérification des certificats.


{{site.data.keyword.iot_short_notm}} requiert TLS V1.2 et les suites de chiffrements ci-dessous :
- ECDHE-RSA-AES256-GCM-SHA384
- AES256-GCM-SHA384
- ECDHE-RSA-AES128-GCM-SHA256
- AES128-GCM-SHA256

## Authentification client MQTT
{: #mqtt_authentication}

**Important :** Chaque client MQTT requiert un ID de client unique. Si vous tentez de connecter un client dans votre organisation à l'aide d'un ID de client déjà connecté, la première connexion est déconnectée. 

Les terminaux et les passerelles qui sont directement connectés à {{site.data.keyword.iot_short_notm}} affichent une icône de statut sur le tableau de bord pour indiquer qu'ils sont connectés. Les terminaux qui sont indirectement connectés via une passerelle sont affichés comme des terminaux étant déconnectés car le tableau de bord ne reconnaît pas les terminaux qui sont connectés via une passerelle. 

### Identificateurs de client MQTT

Pour que les terminaux, les applications et les passerelles puissent s'authentifier, définissez chaque client MQTT en utilisant les identificateurs de client et le format suivants : 

|Type de client |ID|Format d'ID MQTT|
|:---|:---|:---|
|Applications|a|<pre class="pre">a:<var class="keyword varname">orgId</var>:<var class="keyword varname">appId</var></pre>
|Applications évolutives|A|<pre class="pre">A:<var class="keyword varname">orgId</var>:<var class="keyword varname">appId</var></pre>
|Terminaux|d|<pre class="pre">d:<var class="keyword varname">orgId</var>:<var class="keyword varname">deviceType</var>:<var class="keyword varname">deviceId</var></pre>|
|Passerelles|g|<pre class="pre">g:<var class="keyword varname">orgId</var>:<var class="keyword varname">typeId</var>:<var class="keyword varname">deviceId</var></pre>|

Où
- *orgId* est l'ID d'organisation unique composé de six caractères ayant été généré lorsque vous avez enregistré le chaîne. 
- *appId* est un identificateur de chaîne unique défini par l'utilisateur pour le client. 
- *deviceId* identifie un terminal ou une passerelle de façon unique parmi tous les types de terminal et de passerelle, et s'apparente à un numéro de série. 
- *device_type* est l'identificateur du type de terminal qui se connecte, et s'apparente à un numéro de modèle. 
- *typeId* est l'identificateur du type de passerelle qui se connecte, et s'apparente à un numéro de modèle. 

Les valeurs pour *appId*, *type_id*, *device_type* et *device_id* ne doivent pas dépasser 36 caractères et ne peuvent contenir que les caractères suivants : 
- Caractères alphanumériques (a-z, A-Z, 0-9)
- Tirets (-)
- Traits de soulignement (_)
- Points (.)

**Remarques :**
- Lorsque vous vous connectez au service Quickstart, l'authentification n'est pas obligatoire. 
- Vous n'avez pas besoin d'enregistrer une application avant de vous connecter.


### Connexion d'applications à l'aide de MQTT

Les applications {{site.data.keyword.iot_short_notm}} requièrent une clé d'API pour se connecter à une organisation. Lorsqu'une clé d'API est enregistrée, un jeton est généré et doit être utilisé avec cette clé d'API. 

Le code suivant fournit un exemple de clé d'API :

<pre class="pre">a-<var class="keyword varname">orgId</var>-a84ps90Ajs</pre>
{: codeblock}

L'exemple suivant illustre un jeton d'authentification standard :

```
 MP$08VKz!8rXwnR-Q*
```

Lorsque vous établissez une connexion MQTT à l'aide d'une clé d'API, prenez soin de respecter les exigences suivantes :

- L'ID de client MQTT doit être au format suivant : a:*orgId*:*appId*
- Le nom d'utilisateur MQTT doit correspondre à la clé d'API, par exemple, a-*orgId*-a84ps90Ajs
- Le mot de passe MQTT doit correspondre au jeton d'authentification, par exemple, MP$08VKz!8rXwnR-Q*

Pour plus d'informations, voir [Connectivité MQTT pour les applications](../../applications/mqtt.html).

### Authentification de terminal

#### Noms d'utilisateur
Le service {{site.data.keyword.iot_short_notm}} ne prend en charge que l'authentification basée sur un jeton pour les terminaux ; par conséquent, chaque terminal ne possède qu'un seul nom d'utilisateur valide.
La valeur `use-token-auth` indique au service que le jeton d'authentification pour la passerelle ou le terminal est utilisé comme mot de passe pour la connexion MQTT. 

Pour plus d'informations, voir [Connectivité MQTT pour les terminaux](../../devices/mqtt.html)

#### Mots de passe
Si le client utilise l'authentification basée sur un jeton, soumettez le jeton d'authentification de terminal comme mot de passe pour toutes les connexions MQTT. 
