---

copyright:
  years: 2016

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Introdução ao {{site.data.keyword.iot_short_notm}} Starter
{: #gettingstartedtemplate}
<!-- Provide and appropriate ID above -->
*Última atualização: 27 de junho de 2016*
{: .last-updated}

Introdução ao {{site.data.keyword.iot_full}} usando o modelo do Starter do {{site.data.keyword.iot_short_notm}}. Com o Starter, é possível simular rapidamente um dispositivo, criar
cartões, gerar dados e iniciar a análise e a exibição de dados no painel do {{site.data.keyword.iot_short_notm}}.
{: shortdesc}

O Starter implementa automaticamente e conecta esses serviços:

{{site.data.keyword.iot_short_notm}} - A plataforma fornece um kit de ferramentas de IoT versátil que inclui dispositivos de gateway, gerenciamento de dispositivo e acesso poderoso ao
aplicativo. Usando o {{site.data.keyword.iot_short_notm}}, é possível coletar dados do dispositivo conectados e executar analítica em dados em tempo real a partir de sua organização.

{{site.data.keyword.sdk4nodefull}} - cria um ambiente de tempo de execução no qual o Node-RED é executado.

{{site.data.keyword.cloudantfull}} - um banco de dados no qual o Node-RED armazena metadados.

## Sobre {{site.data.keyword.iot_short_notm}}
{: #about_iotplatform}
O {{site.data.keyword.iot_short_notm}} fornece acesso poderoso ao aplicativo para dispositivos de IoT e dados para ajudá-lo a rapidamente compor aplicativos de analítica, painéis de visualização e
aplicativos móveis de IoT.
O {{site.data.keyword.iot_short_notm}} permite executar operações de gerenciamento de dispositivo poderosas e armazenar e acessar dados do dispositivo, conectar uma ampla variedade de dispositivos e
dispositivos de gateway. O {{site.data.keyword.iot_short_notm}} fornece comunicação segura para e a partir de seus dispositivos usando MQTT e TLS.

## Sobre o Node-RED
{: #about_nodered}
O Node-RED é uma ferramenta para conexão de dispositivos de hardware, APIs e serviços on-line em conjunto, de maneiras novas e interessantes.  É possível usar o Node-RED para criar um termostato simulado que
envia dados simulados para o seu serviço do {{site.data.keyword.iot_short_notm}}. É possível criar cartões para exibir dados em tempo real no painel do {{site.data.keyword.iot_short_notm}}. Para obter mais informações, consulte a [documentação do Node-RED](https://console.ng.bluemix.net/docs/starters/Node-RED/nodered.html#nodered).

## Familiarização
{: #gettingaround}
Conforme você executa as etapas a seguir, você utilizará três guias diferentes em seu navegador:
  1. *Painéis do {{site.data.keyword.Bluemix_notm}}* - Use o painel do {{site.data.keyword.Bluemix_notm}} para ver o estado de sua implementação, ler a documentação e
ativar os painéis.
  2. *Painel do {{site.data.keyword.iot_short_notm}}* - Use o painel para trabalhar com este serviço. Você usará o painel para definir tipos de dispositivos, registrar
dispositivos, monitorar dados de sensor de entrada, criar cartões de visualização de dados e ver visualizações de dados em tempo real.
  3. *Node-RED* - Executando como um aplicativo da web node.js, você irá configurar e executar o fluxo do simulador de dispositivo e trabalhar com outros fluxos para processar dados a
partir do {{site.data.keyword.iot_short_notm}}.

## Definindo um dispositivo simulado em {{site.data.keyword.iot_short_notm}}
{: #definingsimdev}
Registre o seu dispositivo com o {{site.data.keyword.iot_short_notm}}:
  1.	No {{site.data.keyword.Bluemix_notm}}, acesse a guia de visão geral para o seu aplicativo implementado.
  2.	Clique na caixa do {{site.data.keyword.iot_short_notm}} na seção ou guia do Connections.
  3.	Clique em Ativar no painel para abrir o painel do {{site.data.keyword.iot_short_notm}} em uma nova janela do navegador.
  4.	Selecionar dispositivos
  5.	Clique em Incluir dispositivo
  6.	Clique em Criar tipo de dispositivo no diálogo Incluir dispositivo.
  7.	Clique em Criar tipo de dispositivo no diálogo Criar tipo de dispositivo.
  8. Insira um nome descritivo e uma descrição para o tipo de dispositivo, como termostato.
  9.	Ignorar: Definir Modelo.
  10.	Ignorar Metadados.
  11.	Clique em Criar para incluir o novo tipo de dispositivo.
  12.	Clique em Avançar para incluir o seu dispositivo (Escolher tipo de dispositivo é pré-selecionado).
  13.	Insira um ID do dispositivo como LivingRoomThermo1.
  14.	Ignorar: Inserir metadados de dispositivo.
  15.	Clique em Avançar para incluir uma conexão de dispositivo com um token de autenticação gerado automaticamente.
  16.	Verifique se as informações de resumo estão corretas e, em seguida, clique em Incluir para incluir a conexão.
  17.	Na página de informações sobre o dispositivo que é aberta, copie e salve as informações sobre o dispositivo:
      -	ID da organização
      -	Device Type
      -	Device ID
      - Método de
autenticação
      - Token de autenticação

**Dica**: você precisará do ID da organização, do tipo de dispositivo e do ID de dispositivo nas próximas poucas etapas para finalizar a configuração do aplicativo Node-RED para
concluir a conexão.

## Configurando e executando o simulador de dispositivo do Node-RED.
{: #confignodered}
Configure o simulador de dispositivo do Node-RED. Use o simulador de dispositivo para enviar mensagens do dispositivo de MQTT para {{site.data.keyword.iot_short_notm}}. O simulador de dispositivo
envia informações de temperatura e umidade para o {{site.data.keyword.iot_short_notm}}.

1. Em seu painel do Bluemix, abra o Node-RED selecionando **Visualizar aplicativo** ou clicando no link (por exemplo, http:// myIoTApp.mybluemix.net).
2.	Clique em **Acessar o seu editor de fluxo do Node-RED** para abrir o editor.
3.	Dê um clique duplo no nó azul **Enviar** para {{site.data.keyword.iot_short_notm}} no fluxo de Simulador de Dispositivo.
  1.	Verifique se a Autenticação = Bluemix Service
  2.	Cole o Tipo de dispositivo a partir do dispositivo que você registrou.
  3.	Cole o ID do dispositivo a partir do dispositivo que você registrou.
  4.	Clique em OK.
4.	No canto superior direito do editor de fluxo do Node-RED, clique em **Implementar**.
5.	Configure o fluxo do Monitor de Temperatura do Node-RED
  1.	Dê um clique duplo no nó azul do IBM IoT App In no fluxo Simulador do dispositivo.
  2.	Mude a Autenticação para Bluemix Service
  3.	Selecione Todos para Tipo de dispositivo, ID do dispositivo, evento e formato
  4.	Clique em OK.
6.	No canto superior direito do editor de fluxo do Node-RED, clique em **Implementar**.
7.	Valide a conexão de dispositivo
  1.	Em sua outra guia ou janela do navegador, retorne para o painel do {{site.data.keyword.iot_short_notm}}.
  2.	Selecione Dispositivos e clique em LivingRoomThermo1 ou no nome do dispositivo que você incluiu, se diferente.  
  A página de informações sobre o dispositivo é aberta. Esta visualização permite ver o status da conexão para o seu dispositivo. O status do dispositivo está desconectado.
  3.	De volta em seu editor de fluxo do Node-RED, clique no botão no nó cinza Enviar Dados para gerar uma carga útil de ativos.
  A carga útil contém os pontos de dados a seguir:
  ```
  {"d":{"temp":15,"umidade":50,"local":{"longitude":-98.49,"latitude":29.42}}}
  ```
  4.	Na guia de depuração da área de janela direita, verifique se as mensagens são criadas.
  5.	Na página de informações sobre o dispositivo do {{site.data.keyword.iot_short_notm}}, verifique se você vê os mesmos pontos de dados recebidos do dispositivo na seção Informações do Sensor.

8.	Conecte o seu dispositivo de IoT de amostra ao {{site.data.keyword.iot_short_notm}} para visualizar dados do dispositivo.

## Criando cartões no {{site.data.keyword.iot_short_notm}} para mostrar dados ativos
{: #createcards}
Para criar cartões, execute as tarefas a seguir.

### Gere automaticamente dados a cada 3 segundos
1. Na guia do Node-RED, dê um clique duplo no nó Enviar dados
2.	Configure Repetir a cada Intervalo de 3 segundos
3.	Implementação

### Crie novos cartões no painel
1. Acesse a guia do navegador de painel do {{site.data.keyword.iot_short_notm}}.
2. Selecione PLACAS.
3. Clique em Criar nova placa.
4. Dê-lhe um nome como Ambiente inicial.
5. Crie.
6. Clique em Incluir novo cartão (para temperatura).
   1.	Sob Dispositivos, selecione Gráfico em tempo real.
   2.	Dados de origem de cartão, verifique o seu Dispositivo "LivingRoomThermo1" e, em seguida, clique em Avançar.
   3. Conecte o novo conjunto de dados.
       - Nome: Temperatura
       - Evento: atualização
       - Propriedade: temp
       - Tipo: Valor flutuante
       - Unidade: °C
       - Precisão: 2
       - Mín: 0
       - Máx: 50
       - Avançar
  4. Visualização de cartão.
       - Selecionar L
       - Avançar
  5. Informações de cartão.
       - Título: Temperatura
  6.	Submeter.
  7.	O cartão de temperatura parece no painel e inclui um gráfico dos dados de temperatura em tempo real.
7.	Clique em Incluir novo cartão (para umidade).
  1.	Sob Dispositivos, selecione Mostrar mais.
  2.	Selecione Gráfico.
  3.	Dados de origem de cartão, verifique o seu Dispositivo e, em seguida, clique em Avançar.
  4.	Conecte o novo conjunto de dados.
       -	Nome: Umidade
       -	Evento: atualização
       -	Propriedade: umidade
       -	Tipo: Valor flutuante
       -	Unidade: "% de umidade relativa"
       -	Precisão: 1
       -	Mín: 10
       -	Máx: 95
       -	Avançar
  5.	Visualização de cartão.
       -	Configurações
          -	Limite inferior: 40 e justo
          -	Meio: bom
          -	Limite máximo: 50% e justo
       -	Selecione M
       -	Avançar
  6.	Informações de cartão.
       -	Título: Umidade
  7.	Submeter.
  8.	O cartão de umidade aparece no painel e inclui um gráfico que mostra os dados de umidade em tempo real.
8.	Clique em Incluir novo cartão (para local).
  1.	Sob Dispositivos, selecione Mostrar mais.
  2.	Selecionar Valor.
  3.	Dados de origem de cartão, verifique o seu Dispositivo. "LivingRoomThermo1" e, em seguida, clique em Avançar.
  4.	Conecte o novo conjunto de dados.
       -	Nome: Latitude
       -	Evento: atualização
       -	Propriedade: location.latitude
       -	Tipo: Valor flutuante
       -	Unidade: "°N"
       -	Precisão: 2
       -	Mín: -180
       -	Máx: 180
  5. Conecte o novo conjunto de dados.
       -	Nome: Longitude
       -	Evento: atualização
       -	Propriedade: location.longitude
       -	Tipo: Valor flutuante
       -	Unidade: "°E"
       -	Precisão: 2
       -	Mín: -180
       -	Máx: 180
       -  Avançar
  6.	Visualização de cartão.
       -	Selecionar L
       -	Avançar
  7.	Informações de cartão.
       -	Título: Local
  8.	Submeter.
9.	Veja a atualização de novos cartões em tempo real com os dados do simulador gerados pelo fluxo do Node-RED.
**Nota**: o Node-RED continua a enviar dados até que você pare.
10.	No editor de fluxo do Node-RED, atualize o nó Enviar Dados para Repetir: nenhum.
11.	Implementação.

## O Que Vem a Seguir?
{: #whatsnext}
Agora que o seu dispositivo simulado está enviando dados para o {{site.data.keyword.iot_short_notm}}, é possível continuar a interagir em seu projeto de IoT.

1. [Procure as suas Receitas de IoT](https://developer.ibm.com/recipes/?post_type=tutorials&s=watson+iot) para conectar um dispositivo físico como um Raspberry Pi e envie dados para o
{{site.data.keyword.iot_short_notm}}.
2. [Implemente um aplicativo node.js de amostra para visualizar dados do dispositivo.](https://www.bluemix.net/docs/services/IoT/visualizingdata_sample.html)
3.	Proteja por senha o editor de fluxo do Node-RED. Por padrão, o editor é aberto para que qualquer pessoa acesse e modifique os fluxos. Para proteger por senha o editor, execute as tarefas a seguir:
  1.	No painel do Bluemix, selecione a página 'Variáveis de Ambiente' para o seu aplicativo.
  2.	Inclua as variáveis definidas pelo usuário a seguir:
       -	NODE_RED_USERNAME - o nome do usuário para proteger o editor
       -	NODE_RED_PASSWORD - a senha para proteger o editor
  3.	Clique em salvar.


# Links Relacionados
{: #rellinks}

## Tutoriais e amostras
{: #samples}
* [Receitas para conectar os seus dispositivos](https://developer.ibm.com/recipes/?post_type=tutorials&s=iot){:new_window}
* [{{site.data.keyword.iot_short}} Reproduzir organização](https://play.internetofthings.ibmcloud.com/){:new_window}
* [Conectando um Intel Galileo ao {{site.data.keyword.iot_short}}](https://developer.ibm.com/recipes/tutorials/connect-an-intel-galileo-to-the-internet-of-things-foundation-connect/){:new_window}
* [Conectando um ARM&reg; mbed&trade; Kit Starter do IoT](https://developer.ibm.com/recipes/tutorials/arm-mbed-iot-starter-kit-part-1/){:new_window}
* [Conectando um Raspberry Pi ao {{site.data.keyword.iot_short}}](https://developer.ibm.com/recipes/tutorials/raspberry-pi-4/){:new_window}

## Referência de API
{: #api}
* [ Documentação da API do {{site.data.keyword.iot_short}}](https://docs.internetofthings.ibmcloud.com/swagger/v0002.html#/){:new_window}


## Links Relacionados
{: #general}
* [Documentação do {{site.data.keyword.iot_short_notm}}](https://www.bluemix.net/docs/services/IoT/iotplatform_overview.html){:new_window}
* [Documentação do iniciador do {{site.data.keyword.sdk4nodefull}}](https://console.ng.bluemix.net/docs/starters/Node-RED/nodered.html){:new_window}
