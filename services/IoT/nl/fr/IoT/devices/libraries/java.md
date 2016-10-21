---

copyright :
  années : 2015, 2016

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Java pour les développeurs de terminaux
{: #java}

Dernière mise à jour : 2 août 2016
{: .last-updated}


Vous pouvez utiliser Java pour générer et personnaliser des terminaux qui interagissent avec votre organisation sur {{site.data.keyword.iot_full}}. Utilisez les informations et les exemples fournis pour commencer à développer vos terminaux à l'aide de Java. {:shortdesc}

## Téléchargement du client et des ressources Java
{: #java_client_download}

Pour accéder aux bibliothèques et exemples client Java pour {{site.data.keyword.iot_short_notm}}, accédez au référentiel [iot-java](https://github.com/ibm-watson-iot/iot-java) dans GitHub et exécutez les instructions d'installation. 


## Constructeur
{: #constructor}

Le constructeur génère l'instance client et accepte un objet de propriétés contenant les définitions suivantes : 

|Définition |Description |
|:---|:---|
|`org` |ID de votre organisation. Cette zone est obligatoire. Si vous utilisez un flux Quickstart, spécifiez `quickstart`.|
|`type`  |Type de votre terminal. Cette zone est obligatoire. |
|`id`  |ID de votre terminal. Cette zone est obligatoire. |
|`auth-method`   |Méthode d'authentification à utiliser. La seule valeur actuellement prise en charge est `token`.|
|`auth-token`   |Jeton d'authentification permettant d'établir une connexion sécurisée entre votre terminal et Watson IoT Platform.|
|`clean-session`|Valeur true ou false requise uniquement si vous souhaitez connecter l'application en mode d'abonnement durable. Par défaut, `clean-session` prend la valeur `true`.|

**Remarque :** Pour connecter le terminal en mode d'abonnement durable, affectez la valeur `false` à l'option `clean-session`. Pour plus d'informations sur l'option 'clean-session', voir la section 'Subscription Buffers and Clean Session' de la [documentation MQTT](../../reference/mqtt/index.html#subscription-buffers-and-clean-session).

L'objet de propriétés crée des définitions qui sont utilisées pour interagir avec le module {{site.data.keyword.iot_short_notm}}. 

Le code suivant illustre un terminal qui publie des événements en mode Quickstart. 

```
package com.ibm.iotf.sample.client.device;

import java.util.Properties;

import com.google.gson.JsonObject;
import com.ibm.iotf.client.device.DeviceClient;

public class QuickstartDeviceEventPublish {

    public static void main(String[] args) {

        //Provide the device specific data using Properties class
        Properties options = new Properties();
        options.setProperty("org", "quickstart");
        options.setProperty("type", "iotsample-arduino");
        options.setProperty("id", "00aabbccde03");

        DeviceClient myClient = null;
        try {
            //Instantiate the class by passing the properties file
            myClient = new DeviceClient(options);
        } catch (Exception e) {
            e.printStackTrace();
        }

        //Connect to the {{site.data.keyword.iot_short_notm}}
        myClient.connect();

        //Generate a JSON object of the event to be published
        JsonObject event = new JsonObject();
        event.addProperty("name", "foo");
        event.addProperty("cpu",  90);
        event.addProperty("mem",  70);

        //Quickstart flow allows only QoS = 0
        myClient.publishEvent("status", event, 0);
        System.out.println("SUCCESSFULLY POSTED......");

  ...
```

L'exemple de code suivant montre comment les terminaux peuvent publier des événements dans un flux enregistré. 


```
package com.ibm.iotf.sample.client.device;

import java.util.Properties;

import com.google.gson.JsonObject;
import com.ibm.iotf.client.device.DeviceClient;

public class RegisteredDeviceEventPublish {

    public static void main(String[] args) {

        //Provide the device specific data, as well as Auth-key and token using Properties class
        Properties options = new Properties();

        options.setProperty("org", "uguhsp");
        options.setProperty("type", "iotsample-arduino");
        options.setProperty("id", "00aabbccde03");
        options.setProperty("auth-method", "token");
        options.setProperty("auth-token", "AUTH TOKEN FOR DEVICE");

        DeviceClient myClient = null;
        try {
            //Instantiate the class by passing the properties file
            myClient = new DeviceClient(options);
        } catch (Exception e) {
            e.printStackTrace();
        }

        //Connect to the {{site.data.keyword.iot_short_notm}}
        myClient.connect();

        //Generate a JSON object of the event to be published
        JsonObject event = new JsonObject();
        event.addProperty("name", "foo");
        event.addProperty("cpu",  90);
        event.addProperty("mem",  70);

        //Registered flow allows 0, 1 and 2 QoS
        myClient.publishEvent("status", event);
        System.out.println("SUCCESSFULLY POSTED......");

  ...
```

### Utilisation d'un fichier de configuration

Au lieu d'utiliser directement l'objet de propriétés, vous pouvez utiliser un fichier de configuration contenant les paires nom-valeur pour les propriétés. Si vous utilisez un fichier de configuration contenant un objet de propriétés, utilisez le format de code suivant : 

```
package com.ibm.iotf.sample.client.device;

import java.io.File;
import java.util.Properties;

import com.google.gson.JsonObject;
import com.ibm.iotf.client.device.DeviceClient;

public class RegisteredDeviceEventPublishPropertiesFile {

    public static void main(String[] args) {
        //Provide the device specific data, as well as Auth-key and token using Properties class
        Properties options = DeviceClient.parsePropertiesFile(new File("C:\\temp\\device.prop"));

        DeviceClient myClient = null;
        try {
            //Instantiate the class by passing the properties file
            myClient = new DeviceClient(options);
        } catch (Exception e) {
            e.printStackTrace();
        }

        //Connect to the {{site.data.keyword.iot_short_notm}}
        myClient.connect();

        //Generate a JSON object of the event to be published
        JsonObject event = new JsonObject();
        event.addProperty("name", "foo");
        event.addProperty("cpu",  90);
        event.addProperty("mem",  70);

        //Registered flow allows 0, 1 and 2 QoS
        myClient.publishEvent("status", event, 1);
        System.out.println("SUCCESSFULLY POSTED......");

  ...
```

Le fichier de configuration de l'application doit posséder le format suivant :

```
    [device]
    org=$orgId
    typ=$myDeviceType
    id=$myDeviceId
    auth-method=token
    auth-token=$token
```

## Connexion à {{site.data.keyword.iot_short_notm}}
{: #connecting_to_iotp}

Connectez-vous à {{site.data.keyword.iot_short_notm}} en appelant la fonction connect. Cette fonction prend un paramètre Booléen facultatif, `autoRetry`, auquel la valeur `true` est affectée par défaut. Le paramètre `autoRetry` permet à la bibliothèque de se reconnecter lorsqu'une erreur MqttException se produit. Notez  que la bibliothèque ne tente pas de se reconnecter lorsque des erreurs MqttSecurityException se produisent en raison de l'utilisation de détails d'enregistrement de terminal incorrects, même si le paramètre `autoRetry` a pour valeur `true`.

```
DeviceClient myClient = new DeviceClient(options);

myClient.connect(true);
```

Une fois la connexion établie avec le service {{site.data.keyword.iot_short_notm}}, le client de terminal peut effectuer des opérations, telles que la publication d'événements et l'abonnement à des commandes de terminal à partir d'une application. 


## Publication d'événements
{: #publishing_events}

Les événements constituent le mécanisme par lequel les terminaux publient des données sur {{site.data.keyword.iot_short_notm}}. Le terminal contrôle le contenu de l'événement et affecte un nom à chaque événement qu'il envoie. 

Lorsqu'un événement est reçu par l'instance {{site.data.keyword.iot_short_notm}}, les données d'identification de l'événement reçu identifient le terminal qui a envoyé l'événement, ce qui signifie qu'un terminal ne peut pas simuler les droits d'accès d'un autre terminal. 

Les événements peuvent être publiés avec n'importe lequel des trois [niveaux de qualité de service (QoS)](../../reference/mqtt/index.html#qos-levels) définis par le protocole MQTT. Par défaut, les événements sont publiés avec le niveau QoS 0.

### Publication d'événements avec le niveau QoS par défaut

```
myClient.connect();

JsonObject event = new JsonObject();
event.addProperty("name", "foo");
event.addProperty("cpu",  90);
event.addProperty("mem",  70);

myClient.publishEvent("status", event);
```


### Augmentation du niveau QoS pour un événement

Vous pouvez augmenter les [niveaux QoS](../../reference/mqtt/index.html#qos-levels) des événements qui sont publiés. La publication des événements dont le niveau QoS est supérieur à zéro peut prendre plus de temps, en raison des informations de réception de confirmation supplémentaires qui sont incluses. 

```
myClient.connect();

JsonObject event = new JsonObject();
event.addProperty("name", "foo");
event.addProperty("cpu",  90);
event.addProperty("mem",  70);

//Registered flow allows 0, 1 and 2 QoS
myClient.publishEvent("status", event, 2);
```

### Publication d'événement à l'aide de HTTP

Outre MQTT, vous pouvez configurer les terminaux pour qu'ils publient des événements sur {{site.data.keyword.iot_short_notm}} à l'aide de HTTP en exécutant les étapes suivantes : 

* Construire une instance DeviceClient à l'aide du fichier de propriétés. 
* Construire un événement à publier. 
* Spécifier le nom d'événement et publier l'événement à l'aide de la méthode `publishEventOverHTTP()`, comme illustré dans le code suivant :

``` sourceCode
DeviceClient myClient = new DeviceClient(deviceProps);

JsonObject event = new JsonObject();
event.addProperty("name", "foo");
event.addProperty("cpu",  90);
event.addProperty("mem",  70);

int httpCode = myClient.publishEventOverHTTP("blink", event);
```

Vous trouverez la totalité du code dans l'exemple de terminal [HttpDeviceEventPublish]. 

En fonction des paramètres définis dans le fichier de propriétés, la méthode `publishEventOverHTTP()` publie l'événement en mode Quickstart ou en mode de flux enregistré. Lorsque l'ID d'organisation défini dans le fichier de propriétés est `quickstart`, la méthode `publishEventOverHTTP()` publie l'événement sur le service Quickstart de l'exemple de terminal au format HTTP normal. Lorsqu'une organisation enregistrée valide est utilisée dans le fichier de propriétés, cette méthode publie toujours l'événement dans HTTPS (HTTP via SSL), de sorte que toutes les communications soient sécurisées. 

Le protocole HTTP fournit une distribution de type 'une fois tout au plus', semblable au niveau de qualité de service 'une fois tout au plus' (QoS 0) du protocole MQTT. Lorsque vous utilisez la distribution de type 'une fois tout au plus' pour publier des événements, l'application doit implémenter une logique de relance au cas où une erreur se produirait. 

[HttpDeviceEventPublish]: https://github.com/ibm-messaging/iot-device-samples/blob/master/java/device-samples/src/main/java/com/ibm/iotf/sample/client/device/HttpDeviceEventPublish.java

## Traitement des commandes
{: #handling_commands}

Lorsque le client du terminal se connecte, il s'abonne automatiquement aux commandes de ce terminal. Pour traiter des commandes spécifiques, vous devez enregistrer une méthode de rappel de commande.
Les messages sont renvoyés en tant qu'instance de la classe Command qui possède les propriétés suivantes : 

| Propriété     |Type de données     | Description|
|----------------|----------------|
|`payload` |java.lang.String| Données du contenu du message. |
|`format`  |java.lang.String| Le format peut être n'importe quelle chaîne, par exemple, JSON.|
|`command`   |java.lang.String|Identifie la commande. |
|`timestamp`   |org.joda.time.DateTime|Date et heure de l'événement. |


``` sourceCode
package com.ibm.iotf.sample.client.device;

import java.util.Properties;
import java.util.concurrent.BlockingQueue;
import java.util.concurrent.LinkedBlockingQueue;


import com.ibm.iotf.client.device.Command;
import com.ibm.iotf.client.device.CommandCallback;
import com.ibm.iotf.client.device.DeviceClient;


//Implement the CommandCallback class to provide the way in which you want the command to be handled
class MyNewCommandCallback implements CommandCallback, Runnable {

    // A queue to hold & process the commands for smooth handling of MQTT messages
    private BlockingQueue<Command> queue = new LinkedBlockingQueue<Command>();

    /**
    * This method is invoked by the library whenever there is command matching the subscription criteria
    */
    @Override
    public void processCommand(Command cmd) {
        try {
            queue.put(cmd);
            } catch (InterruptedException e) {
        }
    }

    @Override
    public void run() {
        while(true) {
            Command cmd = null;
            try {
                //In this sample, we just display the command
                cmd = queue.take();
                System.out.println("COMMAND RECEIVED = '" + cmd.getCommand() + "'\twith Payload = '" + cmd.getPayload() + "'");
            } catch (InterruptedException e) {}
        }
    }
}

public class RegisteredDeviceCommandSubscribe {


    public static void main(String[] args) {

        //Provide the device specific data, as well as Auth-key and token using Properties class
        Properties options = new Properties();

        options.setProperty("org", "uguhsp");
        options.setProperty("type", "iotsample-arduino");
        options.setProperty("id", "00aabbccde03");
        options.setProperty("auth-method", "token");
        options.setProperty("auth-token", "AUTH TOKEN FOR DEVICE");

        DeviceClient myClient = null;
        try {
            //Instantiate the class by passing the properties file
            myClient = new DeviceClient(options);
        } catch (Exception e) {
            e.printStackTrace();
        }

        //Pass the above implemented CommandCallback as an argument to this device client
        myClient.setCommandCallback(new MyNewCommandCallback());

        //Connect to the {{site.data.keyword.iot_short_notm}}
        myClient.connect();
    }
}
```

## Exemples
{: #samples}

Pour obtenir une liste d'exemples de terminal et de gestion des terminaux développés à l'aide de la bibliothèque client Java {{site.data.keyword.iot_short_notm}}, voir le [référentiel GitHub](https://github.com/ibm-messaging/iot-device-samples/tree/master/java).
