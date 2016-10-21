---

copyright:
  years: 2015, 2016

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Conectividade MQTT para aplicativos
{: #mqtt}
Última atualização: 31 de agosto de 2016
{: .last-updated}

MQTT é o protocolo primário que dispositivos e aplicativos usam para se comunicar com o {{site.data.keyword.iot_full}}. Bibliotecas do cliente, informações e amostras são fornecidas para ajudá-lo a conectar e integrar seus aplicativos do {{site.data.keyword.iot_short_notm}}.
{:shortdesc}

## Conexões do cliente
{: #client_connections}

Para obter informações sobre segurança do cliente e como conectar clientes MQTT a aplicativos no {{site.data.keyword.iot_short_notm}}, consulte [Conectando aplicativos, dispositivos e gateways ao {{site.data.keyword.iot_short_notm}}](../reference/security/connect_devices_apps_gw.html).

## Autenticação de MQTT
{: #mqtt_authentication}

Aplicativos do {{site.data.keyword.iot_short_notm}} requerem a chave API (interface de programação de aplicativos) para se conectarem a uma organização. Quando uma chave API (interface de programação de aplicativos) é registrada, um token de autenticação é gerado e ele deve ser usado com essa chave API.

O exemplo a seguir mostra uma chave API (interface de programação de aplicativos) típica:

<pre class="pre">a-<var class="keyword varname">orgId</var>-a84ps90Ajs</pre>
{: codeblock}


O exemplo a seguir mostra um token de autenticação típico:

```
MP$08VKz!8rXwnR-Q*
```

Ao fazer uma conexão MQTT usando uma chave API (interface de programação de aplicativos), assegure que as diretrizes a seguir sejam aplicadas:

- O ID do cliente MQTT está no formato: a:*orgId*:*appId*
- O nome do usuário MQTT é a chave API (interface de programação de aplicativos) (por exemplo, a-*orgId*-a84ps90Ajs)
- A senha MQTT é o token de autenticação (por exemplo, MP$08VKz!8rXwnR-Q*)


## Publicando eventos de dispositivo
{: #publishing_device_events}

Um aplicativo poderá publicar eventos como se eles viessem de qualquer dispositivo registrado, por exemplo:

-  Publicar no tópico iot-2/type/*device_type*/id/*device_id*/evt/*event_id*/fmt/*format_string*

Para portabilidade de dados existentes de um dispositivo para o {{site.data.keyword.iot_short_notm}}, é possível criar um aplicativo para processar os dados e publicá-los no {{site.data.keyword.iot_short_notm}}.

**Importante:** a carga útil da mensagem limita-se a no máximo 131072 bytes. As mensagens que excederem o limite serão rejeitadas.

## Publicando comandos de dispositivo
{: #publishing_device_commands}

Um aplicativo pode publicar um comando para qualquer dispositivo registrado, por exemplo:

-  Publicar no tópico iot-2/type/*device_type*/id/*device_id*/cmd/*command_id*/fmt/*format_string*

## Assinando eventos de dispositivo
{: #subscribe_device_events}

Um aplicativo pode assinar eventos de um ou mais dispositivos, por exemplo:

-  Assinar o tópico iot-2/type/*device_type*/id/*device_id*/evt/*event_id*/fmt/*format_string*

**Nota:** para assinar mais de um tipo de evento ou eventos de mais de um único dispositivo, use o caractere curinga para "qualquer" de MQTT (+) para qualquer um dos componentes a seguir:

- device_type
- device_id
- event_id
- format_string

## Assinando comandos de dispositivo
{: #subscribe_device_commands}

Um aplicativo pode assinar comandos que estão sendo enviados a um ou mais dispositivos, por exemplo:

-  Assinar o tópico iot-2/type/*device_type*/id/*device_id*/cmd/*command_id*/fmt/*format_string*

**Nota:** para assinar mais de um tipo de evento ou eventos de mais de um único dispositivo, use o caractere curinga para "qualquer" de MQTT (+) para qualquer um dos componentes a seguir:

-  device_type
-  device_id
-  cmd_id
-  format_string

## Assinando mensagens de status do dispositivo
{: #subscribe_device_status}

Um aplicativo pode assinar o monitoramento do status de um ou mais dispositivos, por exemplo:

-  Assinar o tópico iot-2/type/*device_type*/id/*device_id*/mon

**Nota:** para assinar atualizações de mais de um dispositivo, use o caractere curinga para "qualquer" de MQTT (+) para qualquer um dos componentes a seguir:

- device_type
- device_id

## Assinando mensagens de status do aplicativo
{: #subscribe_app_status}

Um aplicativo pode assinar o monitoramento do status de um ou mais aplicativos, por exemplo:

- Assinar o tópico iot-2/app/*appId*/mon

**Nota:** para assinar atualizações de todos os aplicativos, use o caractere curinga para "qualquer" de MQTT (+) para o componente *appId*.


## Restrições da Iniciação rápida
{: #quickstart_restrictions}
Se você estiver planejando criar código do aplicativo para uso com o serviço de iniciação rápida, os recursos a seguir não serão suportados na iniciação rápida:

- Publicando comandos
- Assinando comandos
- Usando o caractere curinga para "qualquer" de MQTT (+) nos componentes **deviceType** ou **appId**
- Conexão MQTT por Segurança da Camada de Transporte (TLS)
- Aplicativos escaláveis

## Aplicativos escaláveis
{: #scalable_apps}

Ao ajustar a maneira que seus aplicativos se conectam, é possível tornar seus aplicativos do {{site.data.keyword.iot_short_notm}} mais escaláveis balanceando a manipulação de carga de mensagens de eventos entre diversas instâncias de um aplicativo.

O número de clientes necessários para balanceamento de carga e escalabilidade ideais varia por implementação. Para identificar o número ideal de clientes, você precisa realizar um teste de tensão em seu sistema.

Para ativar o balanceamento de carga, assegure que a assinatura do aplicativo seja não durável e que o ID do cliente da assinatura corresponda ao formato a seguir:

<pre class="pre">A:<var class="keyword varname">orgId</var>:<var class="keyword varname">appId</var></pre>
{: codeblock}

Em que:
-  O caractere **A** indica que o cliente é um aplicativo escalável.
-  *orgId* é o ID da organização exclusivo de seis caracteres que foi gerado quando você registrou a sequência alfanumérica do serviço que foi designada quando o serviço foi primeiramente registrado.
-  *appId* é um identificador de sequência exclusivo definido pelo usuário para o cliente. A sequência pode conter apenas caracteres alfanuméricos (a-z, A-Z, 0-9) e os caracteres especiais hífen (-), sublinhado (_) e ponto (.).

**Importante:**
- Apenas assinaturas não duráveis são suportadas para aplicativos escaláveis.
- O ID do cliente deve iniciar com um caractere **A** maiúsculo para ser designado corretamente como um aplicativo escalável pelo {{site.data.keyword.iot_short_notm}}.
- Outros clientes que fazem parte do aplicativo escalável devem usar o mesmo ID do cliente.


### Como Funciona

O serviço do {{site.data.keyword.iot_short_notm}} estende a especificação do protocolo de sistema de mensagens MQTT V3.1.1 para suportar assinaturas compartilhadas. Assinaturas compartilhadas fornecem recursos de balanceamento de carga para aplicativos. Uma assinatura compartilhada poderá ser necessária se um aplicativo corporativo de backend não puder processar o volume de mensagens que estão sendo publicadas em um espaço de tópico específico. Por exemplo, quando muitos dispositivos publicam mensagens que estão sendo processadas por um único aplicativo, pode ser necessário usar o recurso de balanceamento de carga de uma assinatura compartilhada. O suporte de assinatura compartilhada do {{site.data.keyword.iot_short_notm}} limita-se apenas a assinaturas não duráveis.


**Exemplo**

O cenário a seguir é um exemplo de como um aplicativo escalável funciona no {{site.data.keyword.iot_short_notm}}:

- O cliente um se conecta como **A:abc123:myApplication** e assina todos os eventos de dispositivo, o que significa que o cliente um recebe 100% dos eventos de dispositivo publicados.
- O cliente dois se conecta como **A:abc123:myApplication** e também assina todos os eventos de dispositivo, o que significa que o cliente um e o cliente dois compartilham todos os eventos publicados. A carga de processamento é compartilhada entre o cliente um e o cliente dois.
- O cliente três se conecta como **A:abc123:myApplication** e também assina todos os eventos de dispositivo, o que significa que o cliente um, o cliente dois e o cliente três compartilham a carga de processamento dos eventos.
- O cliente dois e o cliente três depois cancelam a assinatura de todos os eventos de dispositivo, o que significa que apenas o cliente um recebe todos os eventos de dispositivo publicados. Enquanto o cliente dois e o cliente três ainda estão conectados ao serviço, o cliente um recebe 100% dos eventos de dispositivo publicados.
- Quando dois ou mais aplicativos compartilham uma assinatura, as mensagens podem não chegar na ordem na qual foram enviadas. Por exemplo, a mensagem B pode chegar ao cliente um antes da mensagem A chegar ao cliente dois, embora a mensagem A tenha sido enviada primeiro.
