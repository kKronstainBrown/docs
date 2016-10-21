---

copyright:
  years: 2016

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# 边缘分析
{: #edge_analytics}
上次更新时间：2016 年 8 月 1 日
{: .last-updated}

通过边缘分析，可将分析规则触发过程从云移至支持边缘分析的网关，通过执行靠近设备的分析处理，可显著降低云的设备数据流量。
{:shortdesk}

设备将其数据发送到支持边缘分析的网关，在该网关中边缘分析规则对数据进行解析。根据规则及其操作，可能会将关键数据和警报发送到 {{site.data.keyword.iot_full}}，在网关上触发警报，或者将警报写入网关本地的文本文件。

下图说明了 {{site.data.keyword.iot_full}} 边缘分析环境的一般体系结构。
![具有边缘分析体系结构的 IBM Watson IoT Platform](images/architecture_platform_edge.svg "具有边缘分析体系结构的 IBM Watson IoT Platform")

**重要信息：**分析功能已通过 {{site.data.keyword.iotrtinsights_full}} 服务合并。如果 {{site.data.keyword.iot_short_notm}} 组织用作现有 {{site.data.keyword.iotrtinsights_short}} 实例的数据源，那么云分析和边缘分析要到迁移了 {{site.data.keyword.iotrtinsights_short}} 实例后才会启用。在迁移完成之前，请继续使用 {{site.data.keyword.iotrtinsights_short}} 仪表板来满足分析需要。有关更多信息，请参阅 IBM developerWorks 上的 [IBM Watson IoT Platform 博客](https://developer.ibm.com/iotplatform/2016/04/28/iot-real-time-insights-and-watson-iot-platform-a-match-made-in-heaven/){: new_window}，以及查看您现有的 {{site.data.keyword.iotrtinsights_short}} 实例仪表板。  

## 开始之前
{: #byb}

开始之前，请创建边缘和操作：
- 确保网关已连接到 {{site.data.keyword.iot_short}}，并且正在传输设备数据。请参阅[连接网关](gateways/dashboard.html)以获取更多信息。
- 在网关上安装边缘分析代理程序 (EAA)。有关信息，请参阅[安装边缘分析代理程序](gateways/dashboard.html#edge)。</br> **提示：**支持 EAA 的网关以网关设备消息的形式提供 EAA 诊断数据。有关信息，请参阅[边缘分析代理程序诊断度量值](#eaa_metrics)。
- 确保要用作规则中条件的设备属性已映射到模式。请参阅[连接设备](iotplatform_task.html)和[创建模式](im_schemas.html)以获取更多信息。

## 管理边缘规则和操作  
{: #managing_rules}

使用以下步骤管理边缘规则：
- **规则**仪表板用于创建和编辑设备和网关的云以及边缘规则和操作。
- **边缘规则网关**仪表板用于激活、停用、更新和除去网关上的边缘规则。要访问“边缘规则网关”仪表板，请在“规则”仪表板中，单击**管理规则**以获取要管理的边缘规则。有关更多信息，请参阅[激活、停用和管理网关的边缘规则](#manage)。

要获取有关网关所连接设备的边缘规则和警报的概述，请使用以下仪表板：

 |仪表板名称 | 描述 |  
 |:---|:---|  
  |基于规则的分析 | 显示组织的规则，包括边缘规则。其他卡会列出已转发的边缘警报、关联的设备、设备属性和已转发的边缘警报信息。 |  
 |基于设备的分析 | 显示连接到组织的设备。其他卡会显示所选边缘设备的已转发警报、所选设备的信息、设备属性和已转发警报信息。 |

 有关缺省分析仪表板的更多信息，请参阅[使用仪表板和卡可视化实时数据](data_visualization.html#default_boards)。


## 创建边缘规则
{: #rules}

边缘规则是基于条件的决策点，用于使实时设备数据与预定义的阈值或其他属性数据相匹配，以在满足条件时触发边缘操作。

**重要信息：**必须为设备类型创建模式后，才能为该设备类型创建规则。有关信息，请参阅[创建设备类型模式](im_schemas.html)。

要创建规则，请执行以下操作：
1. 在 {{site.data.keyword.iot_short}} 仪表板中，转至**规则**。
2. 单击**创建边缘规则**，为规则命名，提供描述，选择要应用规则的边缘设备类型，然后单击**下一步**。  
3. 设置规则逻辑。
添加一个或多个 IF 条件以用作规则的触发器。可以通过并列行的方式添加条件以将其应用为 OR 条件，也可以通过顺序列的方式添加条件以将其应用为 AND 条件。
**注：**为了能够选择设备属性作为规则的输入，必须将该属性映射到模式。请参阅[创建模式](im_schemas.html)以获取更多信息。**重要信息：**要触发一个条件，以触发使用 AND 以顺序方式组合的两个或更多属性条件，触发数据点必须包含在同一设备消息中。如果数据是在多条消息中收到的，那么不会触发顺序条件。
**示例：**
如果参数值大于指定值，那么可使用简单规则来触发警报：
`temp>80`
如果满足阈值组合条件，那么可使用更复杂的规则来触发警报：
`temp>60 AND capacity>50`   

4. 为规则配置有条件触发要求。
要控制一段时间内为规则触发的警报数和操作数，可以为规则配置有条件触发要求。
**重要信息：**有条件触发将作用于规则中的任何条件。例如，如果某个规则使用 OR 设置了 5 个不同的并行条件，那么每个为 true 的条件都会计入有条件触发器计数。要为规则设置有条件触发，请执行以下操作：
 1. 在规则编辑器中，单击缺省**每次满足条件时触发**链接，以打开设置频率要求对话框。
 2. 选择并配置要用于规则的有条件触发器。
 <ul>
 <li>每次满足条件时触发</li>
 <li>在 M *时间单位*内条件满足 N 次时触发</li>
 </ul>  
有关有条件触发器的更详细描述，请参阅“云分析”部分中的[有条件规则触发](cloud_analytics.html#conditional "有条件触发概述")。
5. 创建或选择在满足规则条件时执行的一个或多个操作。
有关边缘操作的更多信息，请参阅[创建边缘操作](#edge_actions "创建边缘操作")。
示例：操作可以是将设备数据发送到云，或者是将警报发写入本地文件。
3. **可选**：为规则选择警报优先级。
 优先级用于对**基于规则的分析**仪表板中显示的警报分类。缺省优先级为“低”。
6. 对规则满意后，单击**保存**。

规则已创建并已添加到浏览仪表板。现在，可以在打开的**边缘规则网关**仪表板中[激活](#manage)该规则。


## 创建边缘操作
{: #edge_actions}

您可以直接在规则编辑器中创建操作，也可以在“操作”选项卡中创建操作，然后在创建规则时选择这些操作。

要在“操作”选项卡上创建操作，请执行以下操作：
1. 在 {{site.data.keyword.iot_short}} 仪表板中，转至**规则**。
2. 在“规则”仪表板中，选择**操作**选项卡。
2. 单击**创建操作**，为操作命名，提供描述，选择操作类型，然后单击**下一步**。
边缘分析支持两种操作类型：
<dl>
<dt>将事件转发到云</dt>  
<dd>设备事件会发送到 {{site.data.keyword.iot_short}}，在其中可以在仪表板和卡中使用该事件，也可以将该事件用于云分析规则。有关信息，请参阅[与云分析集成](#integrate_with_cloud_analytics)。    
**提示：**使用“将事件转发到云”操作可通过直接在网关设备上过滤掉不太重要的数据，从而减少发送到云的设备数据量。</dd>
<dt>警报</dt>  
<dd>警报在网关设备上创建。</dd>
</dl>
3. 为所选的操作类型提供必需参数。  
<dl>
<dt>将事件转发到云</dt>  
<dd>选择要转发到云的事件数据，并提供要在消息中使用的事件名称。</br>
**提示：**设置仪表板和卡时以及云分析规则时，可以使用事件和属性。</br>
您可以执行以下操作：
<ul>
 <li>包含所有设备属性和虚拟属性
 <li>仅包含模型定义的属性和虚拟属性  
 </ul>
 </dd>
<dt>警报</dt>  
<dd>指定警报消息，并至少为该警报选择一个目标。
 <ul>
 <li>转发到云  
 警报会转发到 {{site.data.keyword.iot_short}}，并在“基于规则的分析”和“基于设备的分析”仪表板中显示。
 <li>发布到网关代理程序
 警报会发布到网关代理程序。代理程序配置将确定警报向用户显示的方式。
 <li>保存到本地文本文件
 警报会附加到网关服务器上的 *IBMEdgeAnalyticsAlerts.csv* 文本文件。
 </ul>
 </dd>
</dl>
4. 单击**确定**以创建新操作。

现在，该操作在规则编辑器中可用。


## 激活、停用和管理网关的边缘规则
{: #manage}

要使规则可触发操作，必须首先在一个或多个网关上将其激活。您可使用**边缘规则网关**仪表板来激活、停用、更新和除去网关上的边缘规则。

要激活边缘规则，请执行以下操作：
1. 在“规则”仪表板中，单击**管理规则**按钮以获取要管理的边缘规则。
在打开的**边缘规则网关**仪表板中，可以看到所有已连接且支持 EAA 的网关的列表。未上传并激活规则的网关的规则状态为*无*。
2. 找到要在其上激活规则的网关，然后从“选择操作”列的菜单中，选择**激活**。
边缘规则会上传到网关。上传完成并且规则激活后，“规则状态”会更改为**活动**。  

现在，规则已在网关上激活，当满足规则条件时，将触发配置的操作。

**提示：**要管理多个网关上的规则，可以选中“网关”列标题旁边的“全选”框。清除不想包含的任何网关，然后从“网关”列顶部的**选择操作**菜单中选取操作。

除了激活规则外，还可以对网关执行以下规则管理操作：

操作 | 描述
--- | ---
激活 | 将规则上传到所选网关并激活规则。规则状态会设置为*活动*。
停用 | 停用所选网关上的规则。规则会保留在网关上，并且可以根据需要重新激活。规则状态会设置为*不活动*。
更新 | 将更新版本的规则上传到所选网关。如果网关的规则状态为*活动（旧）*，使用此操作可使网关处于最新状态。规则状态会设置为*活动*。
除去 | 从所选网关中除去规则。网关的规则状态会还原为*无*。


## 与云分析集成
{: #integrate_with_cloud_analytics}

边缘规则触发的操作在支持 EAA 的网关上运行，使用这些操作可过滤流向云的数据，并将网关生成的警报转发到云，以用于 {{site.data.keyword.iot_short}} 仪表板和卡。  

您还可以使用 {{site.data.keyword.iot_short}} 对从网关发送到云的设备数据执行云分析。如果在边缘规则中使用“`将事件转发到云`”操作，创建的消息可以用作云分析规则的输入，就像用于提供触发边缘规则的数据的设备直接连接到 {{site.data.keyword.iot_short}} 一样。

有关如何创建云分析规则和操作的更多信息，请参阅[云分析](cloud_analytics.html)。

## 边缘分析代理程序诊断度量
{: #eaa_metrics}

已连接且支持 EAA 的网关可将诊断信息作为事件类型为 `gateway_xv-monitor-event` 的设备消息发送。</br> **提示：**您可以使用[云分析](cloud_analytics.html)规则来配置警报操作，例如根据支持 EAA 的网关发回的诊断值通过电子邮件发送通知。例如，可以创建规则来发出警报，以提醒您 `SystemLoad` 是否超过特定阈值。

要查看有关网关状态的信息，请执行以下操作：
1. 在 {{site.data.keyword.iot_short}} 仪表板的菜单侧边栏中，选择**设备**。
2. 单击网关设备以打开设备详细信息页面。
3. 访问网关诊断信息：  
 - 查看**最近的事件**部分，以获取网关发送的最近消息的列表。
 - 查看**诊断日志**部分，以获取任何网关警告和其他诊断消息。
 - 查看**传感器信息**部分，以获取来自网关的详细诊断信息。下表描述了网关设备消息中可能包含的不同属性。


属性 | 描述
--- | ---
`MsgInCount` |向边缘分析代理程序 (EAA) 发送的消息数。
`MsgInRate`、`MsgInRate1Min`、`MessageInRate5Min`、`MsgInRate15Min` 和 `MsgInMeanRate` | 在上一个时间段，每秒向 EAA 发送的估计消息数。</br>**注：**`MsgInRate` 是 `MsgInRate1Min` 的别名。`MsgInMeanRate` 是自启动以来的平均消息速率。
`LastHeartBeat` | 生成上一个脉动信号消息时的时间戳记（以毫秒为单位）。脉动信号消息至少每 10 秒生成一次。
`CurrentTimestamp` | 生成当前监视消息时的时间戳记（以毫秒为单位）。
`IsAlive` | 如果 `LastHeartBeat` 与 `CurrentTimestamp` 之间的差值大于 20 秒，那么此属性为 0。
`BytesOutCount` | EAA 向 {{site.data.keyword.iot_short}} 发送的消息字节数。
`BytesOutRate`、`BytesOutRate1Min`、`BytesOutRate15Min`、`BytesOutRate5Min` 和 `BytesOutMeanRate` | 在上一个时间段，每秒 EAA 向 {{site.data.keyword.iot_short}} 发送的估计消息字节数。</br>**注：**`BytesOutRate` 是 `BytesOutRate1Min` 的别名。`BytesOutMeanRate` 是自启动以来的平均速率。
`BytesInCount` | {{site.data.keyword.iot_short}} 向 EAA 发送的消息数。
`BytesInMeanRate`、`BytesInRate1Min`、`BytesInRate`、`BytesInRate15Min` 和 `BytesInRate5Min` | 在上一个时间段，每秒 {{site.data.keyword.iot_short}} 向 EAA 发送的估计消息字节数。</br>**注：**BytesOutRate 是 BytesOutRate1Min 的别名。BytesOutMeanRate 统计自启动以来的平均速率。
`RuleBytesInCount` |向 EAA 规则引擎核心发送的消息字节数。</br> **注：**如果没有为设备类型设置规则，那么该设备类型的消息不会发送到规则引擎核心。
`RuleBytesInRate5Min`、`RuleBytesInRate`、`RuleBytesInMeanRate`、`RuleBytesInRate1Min` 和 `RuleBytesInRate15Min` | 在上一个时间段，每秒向 EAA 规则引擎核心发送的估计消息字节数。</br> **注：**`RuleBytesInMeanRate` 是自启动以来的平均速率。
`MsgOutCount` | EAA 向 {{site.data.keyword.iot_short}} 发送的消息数。
`MsgOutRate`、`MsgOutMeanRate`、`MsgOutRate1Min`、`MessageOutRate5Min` 和 `MsgOutRate15Min` | 在上一个时间段，每秒 EAA 向 {{site.data.keyword.iot_short}} 发送的估计消息字节数。</br> **注：**`MsgOutRate` 是 `MsgOutRate1Min` 的别名。`MsgOutMeanRate` 是自启动以来的平均速率。
`MsgReducePercent` | 入局和出局消息之间的百分比差值。</br>计算公式如下：`(msgIn - msgOut) / msgIn`
`BytesReducePercent` | 入局和出局字节之间的百分比差值。</br>计算公式如下：`(bytesIn - bytesOut) / bytesIn`
`MsgRateReduce` | 入局和出局消息速率之间的百分比差值。</br>计算公式如下：`(msgInRate - msgOutRate) / msgInRate`
`BytesRateReduce` | 入局和出局消息字节之间的百分比差值。</br>计算公式如下：`(bytesInRate - bytesOutRate) / bytesInRate`
`SystemLoad` | 运行 EAA 的系统的当前系统负载。**注：**仅当 `mpstat` 命令在运行 EAA 的系统上可用时，才会发送 CPU 速率。否则会发送上一分钟的系统负载平均值。</br>“系统负载平均值等于在一段时间内，排队等待可用处理器的可运行实体数与在可用处理器上运行的可运行实体数之和的平均值。平均负载的计算方法是特定于操作系统的，但通常为呈衰减趋势且与时间相关的平均值。如果负载平均值不可用，那么将返回负值。”  
在 ManagementFactory.getOperatingSystemMXBean 的 javadoc 中
`FreeMemory` | 运行 EAA 的 Java 虚拟机 (JVM) 的可用内存字节数。
`MemoryUsed` | EAA 使用的 JVM 内存的字节数。
`InQueueSize` | 排队等待 EAA 处理的消息数。
`RuleNumber` | 在规则引擎核心中定义的规则数。
`ProcessorNumber` | 用于调试用途。规则引擎核心中定义的处理器数。</br>**注：**处理器是规则引擎核心中的最小执行单元。
`DataPointsInWindow` | 在时间窗口中缓存的数据点总数。数据点的字节大小根据其数据类型而有所不同。例如，float/int 数据点大小为 8 字节，而 string 数据点大小根据其长度而变化。在大多数情况下，可以使用以下公式来估算时间窗口的内存使用量：`DataPointsInWindow * 8`。
