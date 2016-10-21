---

copyright:
  years: 2016

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Desenvolvendo contratos inteligentes para integração de blockchain ao {{site.data.keyword.iot_short_notm}}
{: #iotblockchain_link}
Última atualização: 30 de agosto de 2016
{: .last-updated}

Use o {{site.data.keyword.blockchainfull}} e o ambiente de desenvolvimento do Hyperledger para criar e testar seus próprios contratos inteligentes derivados dos contratos de amostra fornecidos pela IBM.
{:shortdesc}

Desenvolva e implemente os contratos inteligentes na forma de executáveis de código da cadeia Golang. Use a integração de blockchain ao {{site.data.keyword.iot_short_notm}} para acionar atualizações de contrato e execução de lógica de negócios com dados do evento de dispositivo e grave um novo estado do livro razão no blockchain para cada transação.

Um ambiente de desenvolvimento de integração de blockchain ao {{site.data.keyword.iot_short_notm}} consiste nos componentes a seguir:
- Organização do {{site.data.keyword.Bluemix_notm}}:
 - Serviço do {{site.data.keyword.iot_short_notm}} com integração de blockchain de IoT ativada
 - Malha de {{site.data.keyword.blockchainfull_notm}}
 - Aplicativo Node-RED executando o simulador de dispositivo IoT
**Nota:** também é possível usar um ambiente Node-RED implementado localmente para executar o simulador.
- Ambiente local:
 - Ambiente de desenvolvimento do Hyperledger para desenvolver e testar o código da cadeia do contrato inteligente. O ambiente inclui o GoLang.
 - UI (interface com o usuário) de monitoramento de blockchain
- Ambiente do GitHub:
 - Repositório do GitHub fornecido pela IBM para contratos inteligentes de amostra
 - Repositório do GitHub para implementar contratos inteligentes na malha de {{site.data.keyword.blockchainfull_notm}}

O diagrama a seguir ilustra o ambiente de desenvolvimento de integração de blockchain do {{site.data.keyword.iot_short_notm}}: ![A arquitetura de integração do {{site.data.keyword.iot_short_notm}} de blockchain de IoT.](images/architecture_contracts.svg "Arquitetura de integração do {{site.data.keyword.iot_short_notm}} de blockchain de IoT")

## Antes de iniciar
{: #byb}

Obtenha uma visão geral de {{site.data.keyword.blockchainfull_notm}}, como se relaciona ao conceito geral de blockchain e o que pode fazer por você:
- [{{site.data.keyword.blockchainfull_notm}}](http://www.ibm.com/blockchain/) no IBM.com.
- [DOCS de {{site.data.keyword.blockchainfull_notm}}](https://console.ng.bluemix.net/docs/services/blockchain/index.html) - Introdução ao serviço de {{site.data.keyword.blockchainfull_notm}}.
- [API (interface de programação de aplicativos) de {{site.data.keyword.blockchainfull_notm}}](https://ibmblockchainapi.mybluemix.net/swagger/ui.html?scheme=http&host=127.0.0.1:3000&basepath=/) - Uma visão geral da API de {{site.data.keyword.blockchainfull_notm}}.
- [{{site.data.keyword.blockchainfull_notm}} for Developers](http://www.ibm.com/blockchain/for_developers.html) - Uma visão geral de como blockchain se ajusta a seu ambiente de desenvolvimento, incluindo passagens com demos em tempo real e código implementável para execução no {{site.data.keyword.Bluemix_notm}}.

## Contratos inteligentes de amostra
{: #samples}

Diversos contratos de amostra estão disponíveis para download a partir de [https://github.com/ibm-watson-iot/blockchain-samples](https://github.com/ibm-watson-iot/blockchain-samples). É possível usar os contratos de amostra como uma base para desenvolver seus próprios casos de uso em código da cadeia implementável:

|Contrato de amostra |Descrição |
|:---|:---|
|[Contrato básico de blockchain](https://github.com/ibm-watson-iot/blockchain-samples/tree/master/simple_contract_hyperledger) |Um contrato que permite rastrear e armazenar dados de ativo do dispositivo no blockchain
|[Contrato de linha comercial de blockchain](https://github.com/ibm-watson-iot/blockchain-samples/tree/master/trade_lane_contract_hyperledger) |Uma versão avançada do contrato básico de blockchain|


## Configurar seu ambiente de {{site.data.keyword.blockchainfull_notm}}
{: #configure_environment}
Antes que seja possível iniciar a implementação e os testes dos contratos inteligentes, deve-se configurar seu próprio ambiente de blockchain.

**Nota:** a integração de blockchain ao {{site.data.keyword.iot_short_notm}} suporta a conexão a malhas de {{site.data.keyword.blockchainfull_notm}} e malhas do Hyperledger. Os exemplos a seguir são baseados no uso de {{site.data.keyword.blockchainfull_notm}}.

1. Crie e configure sua malha de {{site.data.keyword.blockchainfull_notm}}.
A integração de blockchain ao {{site.data.keyword.iot_short_notm}} requer que a malha de {{site.data.keyword.blockchainfull_notm}} gerencie o livro razão de blockchain, contratos inteligentes e a infraestrutura geral de blockchain. A integração de blockchain ao {{site.data.keyword.Bluemix_notm}} usa {{site.data.keyword.blockchainfull_notm}} para gerenciar as cadeias. Se você tiver acesso a um ambiente de {{site.data.keyword.blockchainfull_notm}} existente, será possível usá-lo. Caso contrário, deve-se criar uma instância de {{site.data.keyword.blockchainfull_notm}} a partir do [catálogo](https://console.ng.bluemix.net/catalog/services/blockchain/) do {{site.data.keyword.Bluemix_notm}}.

 1. No Painel de sua conta do {{site.data.keyword.Bluemix_notm}}, clique em **Usar serviços ou APIs (interfaces de programação de aplicativos)**.
 2. Localize a seção experimental do catálogo de serviços e selecione **Blockchain**.
   **Dica:** clique [aqui](https://console.ng.bluemix.net/catalog/services/blockchain/) para acessar diretamente a página de serviço experimental de {{site.data.keyword.blockchainfull_notm}}.
 3. Na página de serviço de {{site.data.keyword.blockchainfull_notm}}, verifique as seleções de Incluir serviço:  
    - Espaço - Se você tiver mais do que o espaço padrão `dev`, verifique se você está implementando o serviço no espaço desejado.
    - App - Deixe desvinculado.
    - Nome do serviço - Como opção, mude o nome do serviço para algo que seja fácil de lembrar. Esse nome é exibido no quadro {{site.data.keyword.blockchainfull_notm}} no painel do {{site.data.keyword.Bluemix_notm}}.
    - Plano selecionado - Selecione o plano grátis. O plano grátis fornece dois peers de validação e uma autoridade de certificação.
  4. Clique em **Criar** para implementar {{site.data.keyword.blockchainfull_notm}} no {{site.data.keyword.Bluemix_notm}}.
  O blockchain é implementado inicialmente com dois nós peers. É possível incluir mais nós conforme necessário.

4. Vincule o {{site.data.keyword.iot_short_notm}} a seu serviço de {{site.data.keyword.blockchainfull_notm}}
Para gravar no blockchain a partir do {{site.data.keyword.iot_short_notm}}, deve-se primeiramente vincular os serviços.
     1. No {{site.data.keyword.Bluemix_notm}}, acesse o Painel
     2. Selecione o espaço no qual você implementou {{site.data.keyword.blockchainfull_notm}}.
     3. Clique no quadro **Blockchain**.
     4. Na área de janela à esquerda, clique em **Credenciais de serviço**.
     5. Selecione um conjunto de credenciais de serviço ou clique em **Incluir credenciais** para criar um novo conjunto de credenciais de serviço e dê-lhe um nome descritivo, como "Integração-ao-IoT-Platform".
     6. Nas credenciais de serviço formatadas por JSON, anote os parâmetros a seguir:  
      - Informações do peer: `api_host` e `api_port`
      - Informações do usuário de tipo 1 (cliente): `username` e `secret`  

      Exemplo de credenciais de serviço:
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
     **Importante:** o usuário selecionado não deve estar previamente registrado com um peer diferente do peer que você selecionou.
     7. Clique em **Voltar ao painel** para retornar ao painel de seu {{site.data.keyword.Bluemix_notm}}.
     8. Selecione o espaço no qual você implementou o {{site.data.keyword.iot_short_notm}}.
     9. Clique no quadro **{{site.data.keyword.iot_short_notm}}**.
     10. Clique em **Ativar** para abrir o painel do {{site.data.keyword.iot_short_notm}}.
     11. No painel do {{site.data.keyword.iot_short_notm}}, selecione **Configurações > Conexões** clicando em ![Configurações.](images/platform_settings.png "Configurações") na barra lateral de menus.
     12. Na seção Extensões, em Blockchain, clique em **Incluir conexão de malha**.
    Os campos de conexão de malha são exibidos automaticamente na página que substitui a tabela.
    **Nota:** a integração de blockchain deve estar ativada para a inclusão de malhas. Par a obter informações, consulte [Blockchain](../../reference/extensions/index.html#blockchain) no tópico Integrações de serviços externos.
     14. Insira as informações a seguir para conectar-se à malha:
      - Nome da malha - Insira um nome para identificar a malha no {{site.data.keyword.iot_short_notm}}.
      - Endereço do peer - Insira o endereço de `api_host`.
      - Número da porta - Insira o número de `api_port` ou o número de `api_port_tls`. Use a porta 80 se sua implementação não usar TLS (Segurança da Camada de Transporte). Use a porta 443 se sua implementação usar TLS (Segurança da Camada de Transporte).
      - Usar TLS (Segurança da Camada de Transporte) - Use Segurança da Camada de Transporte para criptografar a comunicação entre o {{site.data.keyword.iot_short_notm}} e o contrato na malha. Os números de porta padrão são configurados pela instância do {{site.data.keyword.iot_short_notm}} implementada à qual você está se conectando.
      - ID do usuário - Insira a sequência de `username`.
      - Segredo do usuário - Insira a sequência de `secret`.
     15. Clique em **Confirmar todas as mudanças**
A tabela de malha é preenchida com a nova conexão de malha.  

## Criar, testar e implementar seus contratos inteligentes
{: #test_contracts}

Agora é possível criar seu próprio código da cadeia de contrato inteligente em GoLang, testá-lo no ambiente de simulação e implementar e testar o mesmo em sua própria de{{site.data.keyword.blockchainfull_notm}}.

1. Crie um projeto do GitHub para armazenar o código da cadeia de seu contrato inteligente.
Os contratos inteligentes que você deseja implementar devem estar em um repositório público do GitHub. Para obter mais informações, consulte https://github.com/.
2.  Configure um ambiente de desenvolvimento e teste local do Hyperledger.
Para desenvolver e testar seu próprio código da cadeia antes de implementá-lo em {{site.data.keyword.blockchainfull_notm}}, deve-se configurar um ambiente de desenvolvimento local. Esse ambiente inclui GoLang, que é usado para escrever o código da cadeia para seus contratos.
 1. Configurar o ambiente de desenvolvimento.  
 O ambiente de desenvolvimento inclui as ferramentas necessárias para desenvolver seus contratos inteligentes usando a compilação do código da cadeia em GoLang. Para obter mais informações, consulte [Configurando o ambiente de desenvolvimento](https://github.com/hyperledger/fabric/blob/master/docs/dev-setup/devenv.md) na documentação do Hyperledger.
 2. Instale um ambiente de depuração do código da cadeia.
 O ambiente de depuração fornece as ferramentas necessárias para testar e depurar seus contratos inteligentes antes de implementá-los em {{site.data.keyword.blockchainfull_notm}}. Para obter mais informações, consulte [Escrevendo, compilando e executando código da cadeia em um ambiente de desenvolvimento](https://github.com/hyperledger/fabric/blob/master/docs/Setup/Chaincode-setup.md) na documentação do Hyperledger.
 3. Configure uma rede para desenvolvimento.
 A rede para desenvolvimento fornece um ambiente semelhante ao de produção mais estrito para o teste final de seus contratos inteligentes. Use esse ambiente para o teste final de seus contratos testados e depurados antes de implementá-los em {{site.data.keyword.blockchainfull_notm}}. Para obter mais informações, consulte [Configurando uma rede](https://github.com/hyperledger/fabric/blob/master/docs/Setup/Network-setup.md) na documentação do Hyperledger.

3. Opcional: faça download dos contratos inteligentes de amostra fornecidos pela IBM.
A IBM fornece diversos contratos inteligentes dos quais é possível fazer download e usá-los diretamente no estado em que se encontram ou modificá-los para adequação aos objetivos de sua organização.
Para fazer download dos contratos de amostra:
 1. Acesse o repositório do GitHub de amostras de blockchain em: https://github.com/ibm-watson-iot/blockchain-samples/
 As pastas basic_contract_hyperledger e trade_lane_contract_hyperledger contêm, respectivamente, os contratos básicos e de linha comercial.
 3. Use `git clone` no terminal para clonar o projeto https://github.com/ibm-watson-iot/blockchain-samples.
 **Dica:** também é possível fazer download de um arquivo compactado do projeto clicando em **Fazer download do ZIP** na página do projeto.

6. Crie e teste um contrato inteligente.
 Usando a integração de blockchain ao {{site.data.keyword.iot_short_notm}}, é possível fazer upload de contratos inteligentes no formato de executáveis do código da cadeia para {{site.data.keyword.blockchainfull_notm}} para executar a lógica de negócios nos dados do dispositivo gravados no blockchain. O código da cadeia do contrato inteligente é desenvolvido em GoLang.
 O contrato de amostra foca a gravação de dados do dispositivo IoT para eventos de interesse em um blockchain para compartilhamento com terceiros ou para criar entradas de log resistente à violação.
2. Crie os executáveis do contrato.
  O código do contrato deve ser convertido em um executável antes que possa ser implementado no blockchain.
  **Nota:** o contrato de amostra (sample_contract_hyperledger) já está gerado e pode ser implementado no estado em que se encontra.
  Execute as seguintes etapas:
   1. Abra a linha de comandos e navegue até a pasta de contrato.
   2. Execute o comando `go generate`.
   Esse comando executa quaisquer comandos 'go generate' existentes no código. Go generate é uma ferramenta go program que permite a geração de código pré-compilado. Nos contratos de exemplo fornecidos pela IBM, go generate é usado para criar o arquivo schemas.go que esboça o esquema de contrato e arquivo de contrato sample.go.
   **Importante:** o arquivo schemas.go é um componente crítico da integração de blockchain ao {{site.data.keyword.iot_short_notm}}. O arquivo permite que a plataforma confirme se o contrato é compatível com a especificação de integração e permite que o mapeador veja a API (interface de programação de aplicativos) do contrato para a qual os eventos de dispositivos podem ser mapeados.
   2. Execute o comando `go build`.
   Esse comando cria um executável com o mesmo nome que o nome da pasta. O arquivo é o executável do contrato que será implementado no blockchain.

6. Teste o contrato inteligente no ambiente de simulação do Hyperledger.
  Antes de implementar seu contrato inteligente concluído em {{site.data.keyword.blockchainfull_notm}}, será possível testar e depurar o código da cadeia no ambiente de simulação do Hyperledger que você instalou como parte de seu ambiente de desenvolvimento.  

6. Implemente o código da cadeia do contrato inteligente em {{site.data.keyword.blockchainfull_notm}}. Após testar e verificar seu contrato localmente, é possível implementá-lo em sua malha de {{site.data.keyword.blockchainfull_notm}} para testar.
  1. Faça upload de seu contrato para seu repositório público do GitHub.
  Por exemplo, faça upload do arquivo sample.go para:
  `http://github.com/{my organization}/{my project}/`
  2. Registre o contrato com o peer que você conectou anteriormente.
  Use um cliente REST, como CURL ou Postman, para enviar a chamada de registro. Para obter mais informações sobre a chamada de registro, consulte a [Documentação da API (interface de programação de aplicativos) do registro de POST](https://ibmblockchainapi.mybluemix.net/swagger/ui.html?scheme=http&host=127.0.0.1:3000&basepath=/#!/Registrar/registerUser). Use as informações a seguir ao registrar:
  <ul>
  <li>URL: `http://api_host:api_port/registrar`
  <li>Tipo: POST
  <li>Cabeçalho: `Content type: application/x-www-form-urlencoded`
  <li>Carga Útil:  
  ```json
   {  
        "enrollId": "{username}",      
        "enrollSecret": "{secret}"    
   }
   ```

  </ul>
  3. Implemente o contrato no peer.
  Para obter mais informações sobre a chamada de implementação, consulte a [Documentação da API (interface de programação de aplicativos) de devops/implementação de POST](https://ibmblockchainapi.mybluemix.net/swagger/ui.html?scheme=http&host=127.0.0.1:3000&basepath=/#!/Devops/chaincodeDeploy).
  Use as informações a seguir ao implementar:  
  <ul>
  <li>URL: `http://api_host:api_port/devops/deploy`
  <li>Tipo: POST
  <li>Cabeçalho: `Content type: application/x-www-form-urlencoded`
  <li>Carga Útil:  
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
  Seu contrato está implementado na malha.  
  **Importante:** observe o ID do contrato retornado, que está no formato de uma sequência alfanumérica de 128 caracteres. Você precisa do ID do contrato para mapear dispositivos para o contrato.  

10. Mapeie os dados do dispositivo para o novo contrato inteligente.
  Para iniciar a gravação de dados do dispositivo nos novos contratos inteligentes de blockchain, deve-se primeiramente mapear os dados do dispositivo para os contratos.  
   1. No {{site.data.keyword.Bluemix_notm}}, acesse o Painel
   2. Selecione o espaço no qual você implementou o {{site.data.keyword.iot_short_notm}}.
   3. Clique no quadro **{{site.data.keyword.iot_short_notm}}**.
   4. Clique em **Ativar** para abrir o painel do {{site.data.keyword.iot_short_notm}}.
   5. Selecione **Blockchain** clicando em ![Blockchain.](images/platform_blockchain.png "Blockchain") na barra lateral de menus.
   6. Clique em **Vincular contrato**.
   6. Selecione o nome da malha que você criou anteriormente.
   7. Digite as informações a seguir:  
     - ID do contrato - Cole o ID do contrato de 128 caracteres que você salvou ao implementar o contrato.
     - Nome do contrato - Insira um nome para identificar o contrato no {{site.data.keyword.iot_short_notm}}.
     - Selecione o tipo de dispositivo no qual você deseja armazenar dados do dispositivo no blockchain.
     - Selecione o nome do evento para os eventos que você deseja armazenar.
     **Dica:** para localizar os tipos de eventos para um dispositivo, acesse a página **Dispositivos** e clique no nome do dispositivo para abrir a página de detalhes do dispositivo. Role para baixo até a seção **Informações do sensor** para ver uma lista de eventos e pontos de dados disponíveis para o dispositivo.

   11. Mapeie as propriedades do dispositivo disponíveis para os parâmetros do contrato.
   **Importante:** verifique se o tipo de dados para cada ponto de dados mapeado corresponde ao tipo de dados necessário para o parâmetro do contrato para o qual é mapeado.
   Por exemplo, uma propriedade do contrato, como o ID do ativo do tipo sequência deve ser mapeado para uma propriedade do tipo sequência. Os requisitos de parâmetros do contrato são definidos nas definições de `type` no go-code do contrato.
   Por exemplo, o contrato básico fornecido pela IBM tem os requisitos de parâmetros de contrato a seguir:
    <ul>
    <li>  AssetID - sequência
    <li>  Localização - localização geográfica
    <ul>
    <li> Latitude - float64
    <li>  Longitude - float64
    </ul>
    <li>  Temperatura - float64
    <li>  Operadora - sequência
    </ul>  
    Para obter mais informações sobre como mapear dados do dispositivo para contratos, consulte o [Exemplo de mapeamento de dados](https://github.com/ibm-watson-iot/blockchain-samples/wiki/Data-mapping-example) na wiki de amostra de blockchain de IoT no GitHub.
   12. Na página de resumo, verifique se as informações estão corretas.
   13. O mapeamento de dados do dispositivo para o contrato é mostrado na página Blockchain.

7. Teste seu contrato inteligente em {{site.data.keyword.blockchainfull_notm}}.
Para testar seu contrato inteligente, execute um teste de ponta a ponta, criando um dispositivo no {{site.data.keyword.iot_short_notm}}, conectando seu dispositivo ao {{site.data.keyword.iot_short_notm}}, configurando o blockchain de IoT para se conectar à sua malha de blockchain e configurando o {{site.data.keyword.iot_short_notm}} para mapear e armazenar as mensagens de seu dispositivo no blockchain. Usando o console de {{site.data.keyword.blockchainfull_notm}}, é possível visualizar o blockchain para ver os dados do dispositivo no livro razão. Se seu contrato suportar a função readAsset(), será possível usar a UI (interface com o usuário) de monitoramento para visualizar seu blockchain e ver os dados do dispositivo de seu próprio cenário sendo armazenados de maneira indelével em um blockchain.

5. Configure a UI (interface com o usuário) de monitoramento para conectar-se ao {{site.data.keyword.blockchainfull_notm}}.
 **Dica:** se você não tiver instalado a UI (interface com o usuário) de monitoramento em seu ambiente local, será possível fazer isso agora. Siga as instruções do documento leia-me da UI (interface com o usuário) de monitoramento está disponível no diretório do GitHub [UI (interface com o usuário) de monitoramento de blockchain](https://github.com/ibm-watson-iot/blockchain-samples/tree/master/monitoring_ui).
 Acesse as definições de configuração clicando no botão **CONFIGURAÇÃO**. Use as informações a seguir para conectar-se a um contrato:
<table>
<thead>
<tr>
<th>Parâmetro</th>
<th>Value</th>
<th>Comentários</th>
</tr>
</thead>
<tbody>
<tr>
<td>Host e porta da API (interface de programação de aplicativos)</td>
<td>`http://peer_URL:port`</td>
<td>O host e a porta para a API (interface de programação de aplicativos) REST do {{site.data.keyword.blockchainfull_notm}} que tem como prefixo `http://`. Use o endereço de `api_host` e o número de `api_port`. </td>
</tr>
<tr>
<td>ID do código da cadeia</td>
<td>O ID do contrato que foi retornado quando você registrou o contrato.</td>
<td>O ID do contrato é uma sequência alfanumérica de 128 caracteres que corresponde à entrada do ID do contrato.  
**Importante:** ao recortar e colar o ID do contrato, certifique-se de que nenhum espaço seja incluído no ID.Se o ID for inserido incorretamente, as entradas do livro razão de blockchain serão exibidas, mas a função de procura de ativos não funciona.
</td>
</tr>
<tr>
<td>Contexto seguro</td>
<td>O usuário da malha</td>
<td>Esse parâmetro é necessário para se conectar às instâncias do {{site.data.keyword.blockchainfull_notm}} no {{site.data.keyword.Bluemix_notm}}. Use a entrada `secureContext`.  
**Importante:** secureContext deve ser o `username` usado para configurar a malha.
</td>
</tr>
<tr>
<td>Número de blocos a exibir</td>
<td>Um número inteiro positivo. Padrão:   10</td>
<td>O número de blocos de blockchain a exibir.
</td>
</tr>
</tbody>
</table>

3. Na UI (interface com o usuário) de monitoramento, verifique se sua configuração está funcionando conforme esperado.
Use os componentes da UI (interface com o usuário) de monitoramento para interagir com seu contrato de blockchain:  
 - Operações do código da cadeia
 Verifique se as operações do código da cadeia específicas do contrato podem ser executadas conforme esperado. Por exemplo, para o contrato básico, verifique se a execução de uma função `createAsset` resulta na inclusão de um ativo no blockchain.
 - Cargas úteis de respostas
 Verifique se as respostas das solicitações de peer aparecem conforme esperado ao enviar solicitações REST a partir da guia Chaincode Operations.
 - Blockchain
Verifique se blocos são incluídos na cadeia quando você injeta dados de um dispositivo vinculado ou quando usa o componente Chaincode Operations.    

## Próximas etapas
{: #next_steps}

Agora você implementou e explorou os contratos inteligentes de amostra fornecidos pela IBM. No entanto, os contratos básico e de linha comercial fornecem exemplos limitados das muitas possibilidades que se abrem com um código da cadeia de contrato inteligente bem projetado. Agora chegou a hora de experimentar e mapear seus cenários de negócios a contratos do código da cadeia em {{site.data.keyword.blockchainfull_notm}}. Será possível usar, então, o {{site.data.keyword.iot_short_notm}} com a integração de blockchain de IoT para gravar dados do dispositivo no livro razão de blockchain e executar a lógica de negócios armazenada como contratos inteligentes em resposta aos dados.     


Feliz uso de blockchain!
