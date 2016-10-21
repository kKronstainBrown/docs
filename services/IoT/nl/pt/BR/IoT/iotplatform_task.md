---

copyright:
  years: 2016

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Conectando dispositivos
{: #iotplatform_task}
Última atualização: 13 de setembro de 2016
{: .last-updated}

Antes que possa iniciar o recebimento de dados de seus dispositivos IoT, deve-se conectá-los ao {{site.data.keyword.iot_full}}. A conexão de um dispositivo ao {{site.data.keyword.iot_short_notm}} envolve registrar o dispositivo com o {{site.data.keyword.iot_short_notm}} e, em seguida, usar as informações de registro para configurar o dispositivo para se conectar ao {{site.data.keyword.iot_short_notm}}.
{:shortdesc}

## Antes de iniciar
{: #byb}
 
Antes de iniciar o processo de conexão, deve-se assegurar que seus dispositivos atendam os requisitos para comunicação a seguir com o {{site.data.keyword.iot_short_notm}}:

- Seu dispositivo deve ser capaz de se comunicar enviando mensagens do dispositivo no [formato MQTT](reference/mqtt/index.html).
- As mensagens do dispositivo devem se adequar aos requisitos de [carga útil da mensagem](reference/mqtt/index.html#message-payload) do {{site.data.keyword.iot_short_notm}}.

Conclua as etapas a seguir para conectar seu dispositivo ao {{site.data.keyword.iot_short_notm}}.

## Etapa 1: registrando o dispositivo com o {{site.data.keyword.iot_short_notm}}  
{: #iotplatform_subtask1}

O registro de um dispositivo envolve classificar o dispositivo como um tipo de dispositivo, dando ao dispositivo um nome e fornecendo informações do dispositivo. Em seguida, você fornece um token de conexão ou aceita um token que é gerado pelo {{site.data.keyword.iot_short_notm}}.

É possível incluir dispositivos um por vez a partir do painel do {{site.data.keyword.iot_short_notm}} ou usar a [API do {{site.data.keyword.iot_short_notm}}](https://docs.internetofthings.ibmcloud.com/swagger/v0002.html#!/Bulk_Operations/post_bulk_devices_add) para incluir um ou mais dispositivos por vez.

Para incluir um dispositivo a partir do painel do {{site.data.keyword.iot_short_notm}}:

1. Clique no quadro de serviço do {{site.data.keyword.iot_short_notm}} no painel do {{site.data.keyword.Bluemix}} para abrir o painel de serviço ou acesse o painel diretamente usando a URL (Localizador Uniforme de Recursos) a seguir:

 `https://org_id.internetofthings.ibmcloud.com/dashboard/#/overview `

    Em que *org_id* é o ID de sua organização do {{site.data.keyword.Bluemix}}.

2. Na página de serviço, clique em **Ativar painel** para iniciar a administração de sua organização do {{site.data.keyword.iot_short_notm}}.

3. No painel Visão geral, na área de janela de menu, selecione **Dispositivos** e, em seguida, clique em **Incluir dispositivo**.
5. Selecione ou crie um tipo de dispositivo para o dispositivo que você está incluindo.
Cada dispositivo conectado ao {{site.data.keyword.iot_short_notm}} deve estar associado a um tipo de dispositivo. Tipos de dispositivos são grupos de dispositivos que compartilham características.
Quando você incluir seu primeiro dispositivo em sua organização do {{site.data.keyword.iot_short_notm}}, nenhum tipo de dispositivo estará disponível no menu **Tipo de dispositivo**. Você deve primeiramente criar um tipo de dispositivo:
 1. Clique em **Criar tipo de dispositivo**.
 2. Insira um nome, como `my_device_type`, e uma descrição para o tipo de dispositivo.
 3. Opcional: insira atributos e metadados de tipo de dispositivo.
 **Dica:** é possível incluir e editar atributos e metadados posteriormente.
 4. Clique em **Criar** para incluir o novo tipo de dispositivo.
10. Clique em **Avançar** para iniciar o processo de incluir seu dispositivo com o tipo de dispositivo selecionado.
11. Insira um ID do dispositivo. **Dica:** para dispositivos conectados à rede, isso poderia por exemplo ser o endereço de Controle de Acesso à Mídia (MAC) do dispositivo sem dois pontos separando.
O ID do dispositivo é usado para identificar o dispositivo no painel do {{site.data.keyword.iot_short_notm}} e também é um parâmetro necessário para conectar seu dispositivo ao {{site.data.keyword.iot_short_notm}}.
12. Opcional: clique em **Campos adicionais** para incluir informações de dispositivo, como número de série, fabricante, modelo, etc.
 **Dica:** é possível incluir e editar essas informações posteriormente.
12. Opcional: insira os metadados JSON do dispositivo.
 **Dica:** é possível incluir e editar metadados do dispositivo posteriormente.
13. Clique em **Avançar** para concluir a inclusão de seu dispositivo.
14. Verifique se as informações de resumo estão corretas e, em seguida, clique em **Incluir** para incluir a conexão.
**Dica:** você tem a opção para aceitar um token de autenticação gerado automaticamente ou você mesmo fornecer um token de autenticação. Se você optar por criar seu próprio token, certifique-se de que tenha entre oito e 36 caracteres de comprimento, contenha uma combinação de letras minúsculas e maiúsculas, números e hífen, sublinhado ou ponto. O token não deve conter sequências de caracteres repetidos, palavras de dicionário, nomes de usuário ou outras sequências predefinidas.
15. Na página de informações do dispositivo, copie e salve as informações do dispositivo a seguir:  
 - ID da organização, como `tubo8x`
 - Tipo de dispositivo, como `my_device_type`
 - ID do dispositivo, como `my_first_device`
 - Método de autenticação, como `token`
 - Token de autenticação, como `PtBVriRqIg4uh)_-Kl`
**Dica:** Você precisará de ID da organização, Token de autenticação, Tipo de dispositivo e ID do dispositivo para configurar seu dispositivo para se conectar ao {{site.data.keyword.iot_short_notm}}.  

Parabéns, você registrou seu dispositivo. Agora é possível configurar seu dispositivo para se conectar ao {{site.data.keyword.iot_short_notm}}

## Etapa 2: conectando seus dispositivos ao {{site.data.keyword.iot_short_notm}}
{: #iotplatform_subtask2}

Após registrar um dispositivo com o {{site.data.keyword.iot_short_notm}}, será possível usar as informações de registro para conectar o dispositivo e iniciar o recebimento de dados do dispositivo.

O {{site.data.keyword.iot_short_notm}} suporta muitos tipos de dispositivos. O processo básico para conectar um dispositivo normalmente inclui as etapas a seguir:
- Configure seu dispositivo para o sistema de mensagens MQTT e use ID da organização, Token de autenticação, Tipo de dispositivo e ID do dispositivo para autenticar.  
- Envie mensagens do dispositivo para sua organização do {{site.data.keyword.iot_short_notm}} usando o protocolo MQTT.

**Dica:** muitas receitas de conexão estão disponíveis para os dispositivos comumente usados. Para obter uma lista de receitas, consulte as [Receitas de conexão de dispositivo](https://developer.ibm.com/recipes/?post_type=tutorials&s=IoT) que estão disponíveis em IBM.com.

As informações a seguir são necessárias ao conectar seu dispositivo:
- URL (Localizador Uniforme de Recursos): *org_id*.messaging.internetofthings.ibmcloud.com
Em que *org_id* é o ID de sua organização do {{site.data.keyword.iot_short_notm}}.
- 1Port:
 - 1883
 - 8883 (criptografada)
 - 443 (soquetes da web)
- Identificador de dispositivo: d:*org_id*:*device_type*:*device_id*
Essa combinação de parâmetros identifica seu dispositivo de forma exclusiva.
- Nome do usuário: use-token-auth
Esse valor indica que você está usando autorização por token.
- Senha: *Token de autenticação*
Esse valor é o token exclusivo que você definiu ou que foi designado a seu dispositivo quando você o registrou.
- Formato do tópico de evento: iot-2/evt/*event_id*/fmt/*format_string*
 Em que *event_id* especifica o nome do evento que é mostrado no {{site.data.keyword.iot_short_notm}} e *format_string* é o formato do evento, como JSON.
- Formato da mensagem: JSON
 O {{site.data.keyword.iot_short_notm}} suporta diversos formatos, como JSON e text.

Para obter mais informações sobre como conectar seu dispositivo, consulte [Conectividade MQTT para dispositivos](devices/mqtt.html) na documentação técnica.
A seção [Conectividade](https://docs.internetofthings.ibmcloud.com/swagger/v0002.html#!/Connectivity/post_device_types_deviceType_devices_deviceId_events_eventName) da documentação da API (interface de programação de aplicativos) também contém as informações necessárias.
