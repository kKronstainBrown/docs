---

copyright:
  years: 2016

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# 针对 {{site.data.keyword.iot_short_notm}} 区块链集成开发智能合同
{: #iotblockchain_link}
上次更新时间：2016 年 8 月 30 日
{: .last-updated}

使用 {{site.data.keyword.blockchainfull}} 和 Hyperledger 开发环境可创建并测试从 IBM 提供的样本合同派生的自己的智能合同。
{:shortdesc}

开发和部署 Golang 链代码可执行文件格式的智能合同。使用 {{site.data.keyword.iot_short_notm}} 区块链集成可通过设备事件数据来触发合同更新和业务逻辑执行，也可以将新的分类帐状态写入每个交易的区块链。

{{site.data.keyword.iot_short_notm}} 区块链集成开发环境由以下组件组成：
- {{site.data.keyword.Bluemix_notm}} 组织：
 - 启用了 IoT 区块链集成的 {{site.data.keyword.iot_short_notm}} 服务
 - {{site.data.keyword.blockchainfull_notm}} 光纤网
 - 运行 IoT 设备模拟器的 Node-RED 应用程序
 **注：**您还可以使用本地部署的 Node-RED 环境来运行模拟器。
- 本地环境：
 - Hyperledger 开发环境，可开发和测试智能合同链代码。该环境包含 Golang。
 - 区块链监视 UI
- GitHub 环境：
 - IBM 提供的 GitHub 存储库，用于存储样本智能合同
 - 用于将智能合同部署到 {{site.data.keyword.blockchainfull_notm}} 光纤网的 GitHub 存储库

下图显示了 {{site.data.keyword.iot_short_notm}} 区块链集成开发环境：![IoT 区块链 {{site.data.keyword.iot_short_notm}} 集成体系结构](images/architecture_contracts.svg "IoT 区块链 {{site.data.keyword.iot_short_notm}} 集成体系结构")

## 开始之前
{: #byb}

获取 {{site.data.keyword.blockchainfull_notm}} 的概述、IBM Blockchain 与一般区块链概念有着怎样的联系以及具体用途：
- IBM.com 上的 [{{site.data.keyword.blockchainfull_notm}}](http://www.ibm.com/blockchain/)。
- [{{site.data.keyword.blockchainfull_notm}} 文档](https://console.ng.bluemix.net/docs/services/blockchain/index.html) - {{site.data.keyword.blockchainfull_notm}} 服务入门。
- [{{site.data.keyword.blockchainfull_notm}}API](https://ibmblockchainapi.mybluemix.net/swagger/ui.html?scheme=http&host=127.0.0.1:3000&basepath=/) - {{site.data.keyword.blockchainfull_notm}} API 概述。
- [{{site.data.keyword.blockchainfull_notm}} for Developers](http://www.ibm.com/blockchain/for_developers.html) - 概述如何使区块链适合您的开发环境，包括具有实时演示的逐步指南，以及可部署以在 {{site.data.keyword.Bluemix_notm}} 上运行的代码。

## 样本智能合同
{: #samples}

[https://github.com/ibm-watson-iot/blockchain-samples](https://github.com/ibm-watson-iot/blockchain-samples) 上有大量样本合同可供下载。您可以将样本合同用作基础，以将自己的用例开发成可部署的链代码：

|样本合同 |描述 |
|:---|:---|
|[基本区块链合同](https://github.com/ibm-watson-iot/blockchain-samples/tree/master/simple_contract_hyperledger) |此合同允许您跟踪和存储区块链上的设备资产数据
|[贸易航线区块链合同](https://github.com/ibm-watson-iot/blockchain-samples/tree/master/trade_lane_contract_hyperledger) |这是基本区块链合同的高级版本|


## 配置 {{site.data.keyword.blockchainfull_notm}} 环境
{: #configure_environment}
您必须设置自己的区块链环境后，才能开始部署和测试智能合同。

**注：**{{site.data.keyword.iot_short_notm}} 区块链集成支持连接到 {{site.data.keyword.blockchainfull_notm}} 光纤网和 Hyperledger 光纤网。以下示例基于 {{site.data.keyword.blockchainfull_notm}} 的使用。

1. 创建并配置 {{site.data.keyword.blockchainfull_notm}} 光纤网。
{{site.data.keyword.iot_short_notm}} 区块链集成需要 {{site.data.keyword.blockchainfull_notm}} 光纤网来管理区块链分类帐、智能合同和一般区块链基础架构。{{site.data.keyword.Bluemix_notm}} 区块链集成使用 {{site.data.keyword.blockchainfull_notm}} 来管理链。如果您有权访问现有 {{site.data.keyword.blockchainfull_notm}} 环境，那么可以使用 IBM Blockchain。如果无权访问，那么必须从 {{site.data.keyword.Bluemix_notm}} [目录](https://console.ng.bluemix.net/catalog/services/blockchain/)创建 {{site.data.keyword.blockchainfull_notm}} 的实例。

 1. 在 {{site.data.keyword.Bluemix_notm}} 帐户“仪表板”中，单击**使用服务或 API**。
 2. 找到服务目录的试验性部分，然后选择**区块链**。**提示：**单击[此处](https://console.ng.bluemix.net/catalog/services/blockchain/)可直接转至 {{site.data.keyword.blockchainfull_notm}} 试验性服务页面。
 3. 在 {{site.data.keyword.blockchainfull_notm}} 服务页面上，验证“添加服务”选择内容：  
    - 空间 - 如果具有多个缺省 `dev` 空间，请验证是否要将服务部署到目标空间。
    - 应用程序 - 保留未绑定。
    - 服务名称 -（可选）将服务名称更改为容易记住的名称。此名称会显示在 {{site.data.keyword.Bluemix_notm}}“仪表板”的 {{site.data.keyword.blockchainfull_notm}} 磁贴中。
    - 所选套餐 - 选择免费套餐。免费套餐提供两个验证同级和一个认证中心。
  4. 单击**创建**以将 {{site.data.keyword.blockchainfull_notm}} 部署到 {{site.data.keyword.Bluemix_notm}}。区块链初始部署有两个同级节点。可以根据需要添加更多节点。

4. 将 {{site.data.keyword.iot_short_notm}} 链接到您的 {{site.data.keyword.blockchainfull_notm}} 服务
    要从 {{site.data.keyword.iot_short_notm}} 写入区块链，必须首先链接服务。
     1. 在 {{site.data.keyword.Bluemix_notm}} 中，转至“仪表板”。
     2. 选择部署了 {{site.data.keyword.blockchainfull_notm}} 的空间。
     3. 单击**区块链**磁贴。
     4. 在左侧窗格中，单击**服务凭证**。
     5. 选择一组服务凭证，或单击**添加凭证**以创建一组新的服务凭证，并为其提供描述性名称，例如“IoT-Platform-integration”。
     6. 在 JSON 格式的服务凭证中，记录以下参数：  
      - 同级信息：`api_host` 和 `api_port`
      - 类型为 1（客户机）的用户的信息：`username` 和 `secret`  

      服务凭证的示例：
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
     **重要信息：**选择的用户先前不能向除所选同级以外的同级注册过。
     7. 单击**返回到仪表板**以返回到 {{site.data.keyword.Bluemix_notm}}“仪表板”。
     8. 选择部署了 {{site.data.keyword.iot_short_notm}} 的空间。
     9. 单击 **{{site.data.keyword.iot_short_notm}}** 磁贴。
     10. 单击**启动**以打开 {{site.data.keyword.iot_short_notm}} 仪表板。
     11. 在 {{site.data.keyword.iot_short_notm}} 仪表板中，通过单击菜单侧边栏中的 ![设置。](images/platform_settings.png "设置")，选择**设置 > 连接**。
     12. 在“扩展名”部分的“区块链”下，单击**添加光纤网连接**。
    光纤网连接字段会自动显示在该页面上，用于替换表。
    **注：**必须启用区块链集成后才能添加光纤网。有关信息，请参阅“外部服务集成”主题中的[区块链](../../reference/extensions/index.html#blockchain)。
     14. 输入以下信息来连接到光纤网：
      - 光纤网名称 - 输入名称以在 {{site.data.keyword.iot_short_notm}} 中标识光纤网。
      - 同级地址 - 输入 `api_host` 地址。
      - 端口号 - 输入 `api_port` 端口号或 `api_port_tls` 端口号。如果实现未使用 TLS，请使用端口 80。如果实现使用的是 TLS，请使用端口 443。
      - 使用 TLS - 使用传输层安全性对光纤网中 {{site.data.keyword.iot_short_notm}} 与合同之间的通信加密。缺省端口号由要连接的已部署 {{site.data.keyword.iot_short_notm}} 实例进行设置。
      - 用户标识 - 输入 `username` 字符串。
      - 用户密钥 - 输入 `secret` 字符串。
     15. 单击**确认所有更改**
  光纤网表将使用新的光纤网连接进行填充。  

## 创建、测试和部署智能合同
{: #test_contracts}

现在，您可以在 Golang 中创建自己的智能合同链代码，在沙箱环境中对其进行测试，然后在自己的 {{site.data.keyword.blockchainfull_notm}} 光纤网上进行部署和测试。

1. 创建 GitHub 项目以存储智能合同链代码。
要部署的智能合同必须位于公共 GitHub 存储库中。有关更多信息，请参阅 https://github.com/。
2.  设置本地 Hyperledger 开发和测试环境。
要开发和测试自己的链代码后，再将其部署到 {{site.data.keyword.blockchainfull_notm}}，必须设置本地开发环境。此环境包含 Golang，用于编写合同的链代码。
 1. 设置开发环境。
 开发环境包含使用 Golang 中的链代码构建来开发智能合同所需的工具。有关更多信息，请参阅 Hyperledger 文档中的 [Setting up the development environment](https://github.com/hyperledger/fabric/blob/master/docs/dev-setup/devenv.md)。
 2. 安装链代码调试环境。
 调试环境提供了将智能合同部署到 {{site.data.keyword.blockchainfull_notm}} 之前，对其进行测试和调试所需的工具。有关更多信息，请参阅 Hyperledger 文档中的 [Writing, Building, and Running Chaincode in a Development Environment](https://github.com/hyperledger/fabric/blob/master/docs/Setup/Chaincode-setup.md)。
 3. 设置用于开发的网络。
 用于开发的网络提供了更严格的类似生产的环境，用于对智能合同进行最终测试。在将已测试并调试的合同部署到 {{site.data.keyword.blockchainfull_notm}} 之前，使用此环境对这些合同进行最终测试。有关更多信息，请参阅 Hyperledger 文档中的 [Setting Up a Network](https://github.com/hyperledger/fabric/blob/master/docs/Setup/Network-setup.md)。

3. 可选：下载 IBM 提供的样本智能合同。
IBM 提供了大量智能合同，您可以下载并按原样直接使用，也可以根据您组织的目标进行修改。
要下载样本合同，请执行以下操作：
 1. 转至区块链样本 GitHub 存储库：https://github.com/ibm-watson-iot/blockchain-samples/
 basic_contract_hyperledger 和 trade_lane_contract_hyperledger 文件夹分别包含基本合同和贸易航线合同。
 3. 在终端中使用 `git clone` 来克隆 https://github.com/ibm-watson-iot/blockchain-samples 项目。
 **提示：**您还可以通过单击项目页面中的**下载 ZIP** 来下载压缩的项目文件。

6. 创建并测试智能合同。
 通过使用 {{site.data.keyword.iot_short_notm}} 区块链集成，可以将链代码可执行文件格式的智能合同上传到 {{site.data.keyword.blockchainfull_notm}}，以对写入到区块链的设备数据运行业务逻辑。智能合同链代码是使用 Golang 开发的。
 样本合同关注的是如何将相关事件的 IoT 设备数据编写成区块链，以便与第三方共享，或创建防篡改日志条目。
2. 创建合同可执行文件。
  合同代码必须转换为可执行文件后，才能在区块链上进行部署。
  **注：**样本合同 (sample_contract_hyperledger) 已经生成并可按原样进行部署。
  请完成以下步骤：
   1. 打开命令行，并浏览至合同文件夹。
   2. 运行 `go generate` 命令。
   此命令可运行代码中存在的任何“go generate”命令。go generate 是一个 Go 程序工具，允许预构建代码生成。在 IBM 提供的示例合同中，go generate 用于创建 schemas.go 文件，该文件概述合同模式和 sample.go 合同文件。
   **重要信息**：schemas.go 文件是 {{site.data.keyword.iot_short_notm}} 区块链集成的关键组件。该文件允许平台确认合同是否符合集成规范，并允许映射器查看可以将设备事件映射到的目标合同 API。
   2. 运行 `go build` 命令。
   此命令可创建与文件夹名称同名的可执行文件。该文件是将在区块链上部署的合同可执行文件。

6. 在 Hyperledger 沙箱中测试智能合同。
  将完成的智能合同部署到 {{site.data.keyword.blockchainfull_notm}} 之前，可以在作为开发环境一部分安装的 Hyperledger 沙箱中测试并调试链代码。  

6. 将智能合同链代码部署到 {{site.data.keyword.blockchainfull_notm}}。
 在本地测试并验证合同后，可以将其部署到 {{site.data.keyword.blockchainfull_notm}} 光纤网来进行测试。
  1. 将合同上传到公共 GitHub 存储库。
  例如，将 sample.go 文件上传到：
  `http://github.com/{my organization}/{my project}/`
  2. 向早先连接的同级注册合同。
  使用 REST 客户机（例如 CURL 或 Postman）来提交注册调用。有关注册调用的更多信息，请参阅 [POST registrar API 文档](https://ibmblockchainapi.mybluemix.net/swagger/ui.html?scheme=http&host=127.0.0.1:3000&basepath=/#!/Registrar/registerUser)。注册时，请使用以下信息：
  <ul>
  <li>URL：`http://api_host:api_port/registrar`
  <li>类型：POST
  <li>头：`Content type: application/x-www-form-urlencoded`
  <li>有效内容：  
  ```json
   {  
        "enrollId": "{username}",      
        "enrollSecret": "{secret}"    
   }
   ```

  </ul>
  3. 将合同部署到同级。
  有关部署调用的更多信息，请参阅 [POST devops/deploy API 文档](https://ibmblockchainapi.mybluemix.net/swagger/ui.html?scheme=http&host=127.0.0.1:3000&basepath=/#!/Devops/chaincodeDeploy)。部署时，请使用以下信息：  
  <ul>
  <li>URL：`http://api_host:api_port/devops/deploy`
  <li>类型：POST
  <li>头：`Content type: application/x-www-form-urlencoded`
  <li>有效内容：  
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
  您的合同已部署到光纤网。  
  **重要信息：**记录返回的合同标识，格式为 128 个字符的字母数字字符串。您需要合同标识来将设备映射到合同。  

10. 将设备数据映射到新的智能合同。
  要开始将设备数据写入到新的区块链智能合同，必须首先将设备数据映射到合同。  
   1. 在 {{site.data.keyword.Bluemix_notm}} 中，转至“仪表板”。
   2. 选择部署了 {{site.data.keyword.iot_short_notm}} 的空间。
   3. 单击 **{{site.data.keyword.iot_short_notm}}** 磁贴。
   4. 单击**启动**以打开 {{site.data.keyword.iot_short_notm}} 仪表板。
   5. 通过单击菜单侧边栏中的 ![区块链。](images/platform_blockchain.png "区块链")，选择**区块链**。
   6. 单击**链接合同**。
   6. 为早先创建的光纤网选择光纤网名称。
   7. 输入以下信息：  
     - 合同标识 - 粘贴部署合同时保存的 128 个字符的合同标识。
     - 合同名称 - 输入名称以在 {{site.data.keyword.iot_short_notm}} 中标识合同。
     - 选择要在区块链中存储其设备数据的设备类型。
     - 选择要存储的事件的事件名称。
     **提示：**要查找设备的事件类型，请转至**设备**页面，然后单击设备名称以打开设备详细信息页面。向下滚动到**传感器信息**部分，以查看设备的可用事件和数据点的列表。

   11. 将可用设备属性映射到合同参数。
   **重要信息：**验证映射的每个数据点的数据类型是否对应于要将其映射到的合同参数所需的数据类型。
   例如，必须将类型为字符串的合同属性（如“资产标识”）映射到类型为字符串的属性。合同参数需要在合同 go-code 中的 `type` 定义中进行定义。
   例如，IBM 提供的基本合同具有以下合同参数需求：
    <ul>
    <li>  AssetID - 字符串
    <li>  Location - 地理位置
    <ul>
    <li> Latitude - float64
    <li>  Longitude - float64
    </ul>
    <li>  Temperature - float64  
    <li>  Carrier - 字符串
    </ul>  
    有关如何将设备数据映射到合同的更多信息，请参阅 GitHub 上 IoT 区块链样本 Wiki 中的 [Data mapping example](https://github.com/ibm-watson-iot/blockchain-samples/wiki/Data-mapping-example)。
   12. 在摘要页面中，验证信息是否正确。
   13. 设备数据到合同的映射会显示在“区块链”页面中。

7. 在 {{site.data.keyword.blockchainfull_notm}} 中测试智能合同。
为了测试智能合同，请通过在 {{site.data.keyword.iot_short_notm}} 中创建设备，将设备连接到 {{site.data.keyword.iot_short_notm}}，配置 IoT 区块链以连接到区块链光纤网，以及配置 {{site.data.keyword.iot_short_notm}} 以在区块链中映射和存储设备消息，从而执行端到端测试。通过使用 {{site.data.keyword.blockchainfull_notm}} 控制台，可以查看区块链以查看分类帐中的设备数据。如果合同支持 readAsset() 函数，那么可以使用“监视 UI”来查看区块链，并查看要在区域链中持久存储的您自己场景中的设备数据。

5. 配置“监视 UI”以连接到 {{site.data.keyword.blockchainfull_notm}}。
 **提示：**如果尚未在本地环境中安装“监视 UI”，那么可以现在进行安装。按照[区块链监视 UI](https://github.com/ibm-watson-iot/blockchain-samples/tree/master/monitoring_ui) GitHub 目录中提供的“监视 UI”自述文件文档中的指示信息进行操作。
 通过单击**配置**按钮来访问配置设置。
 使用以下信息来连接到合同：
<table>
<thead>
<tr>
<th>参数</th>
<th>值</th>
<th>注释</th>
</tr>
</thead>
<tbody>
<tr>
<td>API 主机和端口</td>
<td>`http://peer_URL:port`</td>
<td>{{site.data.keyword.blockchainfull_notm}} REST API 的主机和端口，前缀为 `http://`。使用 `api_host` 地址和 `api_port` 端口号。</td>
</tr>
<tr>
<td>链代码标识</td>
<td>注册合同时返回的合同标识。</td>
<td>合同标识是对应于合同标识条目的 128 个字符的字母数字字符串。  
**重要信息：**剪切并粘贴合同标识时，请确保标识中不包含任何空格。如果输入的标识不正确，会显示区块链分类帐条目，但资产搜索功能不起作用。
</td>
</tr>
<tr>
<td>安全上下文</td>
<td>您的光纤网用户</td>
<td>此参数对于连接到 {{site.data.keyword.Bluemix_notm}} 上的 {{site.data.keyword.blockchainfull_notm}} 实例是必需的。请使用 `secureContext`条目。  
**重要信息：**secureContext 应该为用于配置光纤网的 `username`。
</td>
</tr>
<tr>
<td>要显示的区块数</td>
<td>正整数。缺省值：10</td>
<td>要显示的区块链区块数。
</td>
</tr>
</tbody>
</table>

3. 在“监视 UI”中，验证您的设置是否在按预期运行。
使用“监视 UI”组件与区块链合同进行交互：  
 - 链代码操作
 验证特定于合同的链代码操作能否按预期运行。例如，对于基本合同，验证运行 `createAsset` 函数是否会生成添加到区块链的资产。
 - 响应有效内容
 验证从“链代码操作”选项卡中提交 REST 请求时，同级请求响应是否按预期显示。
 - 区块链
验证区块是在从链接的设备注入数据时添加到链，还是在使用“链代码操作”组件时添加到链。    

## 后续步骤
{: #next_steps}

现在，您已部署并探索了 IBM 提供的样本智能合同。但是，基本合同和贸易航线合同提供了的示例有限，精心设计的智能合同链代码的实际用途非常广泛。现在，是时候试验业务场景并将其映射到 {{site.data.keyword.blockchainfull_notm}} 中的链代码合同了。随后，您可以将 {{site.data.keyword.iot_short_notm}} 与 IoT 区块链集成一起使用，以将设备数据写入区块链分类帐，并执行存储为智能合同的业务逻辑以响应数据。     


祝您使用区块链顺利！
