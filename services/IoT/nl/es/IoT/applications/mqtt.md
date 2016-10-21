---

copyright:
  years: 2015, 2016

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Conectividad de MQTT para aplicaciones
{: #mqtt}
Última actualización: 31 de agosto de 2016
{: .last-updated}

MQTT es el protocolo primario que utilizan los dispositivos y las aplicaciones para comunicarse con el {{site.data.keyword.iot_full}}. Las bibliotecas de clientes, la información y los ejemplos se proporcionan para ayudarle a conectar y a integrar las aplicaciones de {{site.data.keyword.iot_short_notm}}.
{:shortdesc}

## Conexiones de clientes
{: #client_connections}

Para obtener información sobre la seguridad del cliente y cómo conectar clientes MQTT a las aplicaciones de {{site.data.keyword.iot_short_notm}}, consulte [Conexión de aplicaciones, dispositivos y pasarelas a {{site.data.keyword.iot_short_notm}}](../reference/security/connect_devices_apps_gw.html).

## Autenticación de MQTT
{: #mqtt_authentication}

Las aplicaciones de {{site.data.keyword.iot_short_notm}} requieren una clave de API para conectarse a una organización. Cuando se registra una clave de API, se genera una señal de autenticación, que se debe utilizar con dicha clave de API.

El ejemplo siguiente muestra una clave de API típica:

<pre class="pre">a-<var class="keyword varname">orgId</var>-a84ps90Ajs</pre>
{: codeblock}


El ejemplo siguiente muestra una señal de autenticación típica:

```
MP$08VKz!8rXwnR-Q*
```

Cuando realice una conexión MQTT utilizando una clave de API, asegúrese de que se apliquen las siguientes directrices:

- El ID de cliente MQTT está en el formato: a:*orgId*:*appId*
- El nombre de usuario de MQTT es la clave de la API (por ejemplo, a-*orgId*-a84ps90Ajs)
- La contraseña de MQTT es la señal de autenticación (por ejemplo, MP$08VKz!8rXwnR-Q*)


## Publicación de sucesos de dispositivos
{: #publishing_device_events}

Una aplicación puede publicar sucesos como si procedieran de cualquier dispositivo registrado, como por ejemplo:

-  Publicar en tema iot-2/type/*device_type*/id/*device_id*/evt/*event_id*/fmt/*format_string*

Para portar datos existentes de un dispositivo a {{site.data.keyword.iot_short_notm}}, puede crear una aplicación para procesar los datos y publicarlos en {{site.data.keyword.iot_short_notm}}.

**Importante:** La carga útil de mensajes está limitada a un máximo de 131072 bytes. Los mensajes que superen el límite se rechazarán.

## Publicación de mandatos de dispositivos
{: #publishing_device_commands}

Una aplicación puede publicar un mandato a cualquier dispositivo registrado, como por ejemplo:

-  Publicar en tema iot-2/type/*device_type*/id/*device_id*/cmd/*command_id*/fmt/*format_string*

## Suscripción a sucesos de dispositivos
{: #subscribe_device_events}

Una aplicación puede suscribirse a sucesos de uno o varios dispositivos, como por ejemplo:

-  Suscribirse a tema iot-2/type/*device_type*/id/*device_id*/evt/*event_id*/fmt/*format_string*

**Nota:** Para suscribirse a más de un tipo de suceso o a sucesos de más de un único dispositivo, utilice el carácter comodín "any" de MQTT (+) para cualquiera de los siguientes componentes:

- device_type
- device_id
- event_id
- format_string

## Suscripción a mandatos de dispositivos
{: #subscribe_device_commands}

Una aplicación puede suscribirse a los mandatos que se envían a uno o varios dispositivos, como por ejemplo:

-  Suscribirse a tema iot-2/type/*device_type*/id/*device_id*/cmd/*command_id*/fmt/*format_string*

**Nota:** Para suscribirse a más de un tipo de suceso o a sucesos de más de un único dispositivo, utilice el carácter comodín "any" de MQTT (+) para cualquiera de los siguientes componentes:

-  device_type
-  device_id
-  cmd_id
-  format_string

## Suscripción a mensajes de estado de dispositivos
{: #subscribe_device_status}

Una aplicación puede suscribirse para supervisar el estado de uno o varios dispositivos, por ejemplo:

-  Suscribirse a tema iot-2/type/*device_type*/id/*device_id*/mon

**Nota:** Para suscribirse a actualizaciones desde más de un dispositivo, utilice el carácter comodín "any" de MQTT (+) para cualquiera de los siguientes componentes:

- device_type
- device_id

## Suscripción a mensajes de estado de aplicaciones
{: #subscribe_app_status}

Una aplicación puede suscribirse para supervisar el estado de una o varias aplicaciones, por ejemplo:

- Suscribirse a tema iot-2/app/*appId*/mon

**Nota:** Para suscribirse a actualizaciones para todas las aplicaciones, utilice el carácter comodín "any" de MQTT (+) para el componente *appId*.


## Restricciones de Quickstart
{: #quickstart_restrictions}
Si tiene pensado crear un código de aplicación para su uso con el servicio de Quickstart, las siguientes características no están soportadas en Quickstart:

- Publicación de mandatos
- Suscripción a mandatos
- Utilización del carácter comodín "any" de MQTT (+) en los componentes **deviceType** o **appId**
- Conexión de MQTT a través de Transport Layer Security (TLS)
- Aplicaciones escalables

## Aplicaciones escalables
{: #scalable_apps}

Al ajustar la forma en la que se conectan las aplicaciones, puede convertir sus aplicaciones de {{site.data.keyword.iot_short_notm}} en más escalables equilibrando el manejo de carga de mensajes de sucesos entre varias instancias de una aplicación.

El número de clientes necesarios para un equilibrio de carga óptimo y escalabilidad varía según el despliegue. Para identificar el número óptimo de clientes, tiene que realizar pruebas de carga de su sistema.

Para habilitar el equilibrio de carga, asegúrese de que la suscripción de las aplicaciones no es duradera y de que el ID de cliente de la suscripción coincide con el formato siguiente:

<pre class="pre">A:<var class="keyword varname">orgId</var>:<var class="keyword varname">appId</var></pre>
{: codeblock}

Donde:
-  El carácter **A** indica que el cliente es una aplicación escalable.
-  *orgId* es el ID de organización de seis caracteres exclusivo que se ha generado al registrar la cadena de caracteres alfanumérica del servicio que se ha asignado al registrar por primera vez el servicio.
-  *appId* es un identificador de serie exclusivo definido por el usuario para el cliente. La serie puede contener sólo caracteres alfanuméricos (a-z, A-Z, 0-9) y los caracteres especiales guión (-), subrayado (_), y punto (.).

**Importante:**
- Solo se admiten suscripciones no duraderas con las aplicaciones escalables. 
- El ID de cliente debe empezar por el carácter en mayúscula **A** que debe designar correctamente como una aplicación escalable {{site.data.keyword.iot_short_notm}}.
- Otros clientes que forman parte de la aplicación escalable deben utilizar el mismo ID de cliente.


### Modo de funcionamiento

El servicio de {{site.data.keyword.iot_short_notm}} amplía la especificación de protocolo de mensajería MQTT V3.1.1 para dar soporte a suscripciones compartidas. Las suscripciones compartidas proporcionan funciones de equilibrio de carga para aplicaciones. Es posible que una suscripción compartida sea necesaria si una aplicación de empresa de programa de fondo no puede procesar el volumen de mensajes que se van a publicar en un espacio de tema específico. Por ejemplo, cuando muchos dispositivos publican mensajes que está procesando una única aplicación, es posible que sea necesario utilizar la función de equilibrio de carga de una suscripción compartida. El soporte de la suscripción compartida de {{site.data.keyword.iot_short_notm}} está limitado sólo a suscripciones no duraderas.


**Ejemplo**

El caso de ejemplo siguiente es un ejemplo de cómo funciona una aplicación escalable en {{site.data.keyword.iot_short_notm}}:

- El cliente uno se conecta como **A:abc123:myApplication** y se suscribe a todos los sucesos de dispositivo, lo que significa que el cliente uno recibe el 100 % de los sucesos de dispositivo que se publican.
- El cliente dos se conecta como **A:abc123:myApplication** y también se suscribe a todos los sucesos de dispositivo, lo que significa que el cliente uno y el cliente dos comparten todos los sucesos que se publican. La carga de proceso se comparte entre el cliente uno y el cliente dos.
- El cliente tres se conecta como **A:abc123:myApplication** y también se suscribe a todos los sucesos de dispositivo, lo que significa que el cliente uno, el cliente dos y el cliente tres comparten la carga de proceso para los sucesos.
- El cliente dos y el cliente tres entonces eliminan su suscripción de todos los sucesos de dispositivo, lo que significa que sólo el cliente uno recibe todos los sucesos de dispositivo que se publican. Mientras el cliente dos y el cliente tres siguen estando conectados al servicio, el cliente uno recibe el 100 % de los sucesos de dispositivo que se publican.
- Cuando una o varias aplicaciones comparten una suscripción, es posible que los mensajes no lleguen en el orden en el que se enviaron. Por ejemplo, el mensaje B podría llegar al cliente uno antes de que llegue el mensaje A al cliente dos, aunque el mensaje A se haya enviado en primer lugar.
