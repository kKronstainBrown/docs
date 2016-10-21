---

copyright:
  years: 2016

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# {{site.data.keyword.iot_short_notm}} 故障诊断
{: #ts}
上次更新时间：2016 年 8 月 1 日
{: .last-updated}

此处提供了有关在 {{site.data.keyword.Bluemix_notm}} 上使用 {{site.data.keyword.iot_full}} 的常见故障诊断问题的解答。
{:shortdesc}

## 连接到 {{site.data.keyword.iot_short_notm}} 时发生问题
{: #connection_problem}

您与 {{site.data.keyword.iot_short_notm}} 的连接意外断开。
{:shortdesc}

尝试连接到 {{site.data.keyword.iot_short_notm}} 时，设备或应用程序收到错误。
{: tsSymptoms}

可能有两个设备尝试使用相同的 clientID 和凭证进行连接。每个 clientID 仅允许一个唯一连接。不能有两个并行连接使用相同的标识。应用程序可共享相同 API 密钥，但 MQTT 需要客户机标识始终唯一。
{: tsCauses}

您可以通过确认不会有两个设备尝试使用相同凭证进行连接来解决此问题。
{: tsResolve}

## 设备从 {{site.data.keyword.iot_short_notm}} 间歇性断开连接
{: #disconnection_problem}

您设备到 {{site.data.keyword.iot_short_notm}} 的连接意外间歇性断开。
{:shortdesc}

连接到 {{site.data.keyword.iot_short_notm}} 服务的设备间歇性断开连接。设备会重新连接，但很快再次意外断开连接。
{: tsSymptoms}

可能是因为您在连接时，所使用的 MQTT ping 选项值太低，这导致看上去像连接超时。例如，如果客户机 MQTT 设置不正确，那么不会及时收到 ping，并且连接会关闭。
{: tsCauses}

您可以通过确认为连接正确设置 ping 和 KeepAlive 参数来解决此问题。   
{: tsResolve}


## 获取 {{site.data.keyword.iot_short_notm}} 的帮助和支持
{: #gettinghelp}

如果在使用 {{site.data.keyword.iot_short_notm}} 时遇到问题或疑问，可通过搜索信息或在论坛中进行提问来获取帮助。还可开具支持凭单。

使用论坛进行提问时，请合适地标注您的提问，以方便 {{site.data.keyword.Bluemix_notm}} 开发团队识别。

* 有关使用 {{site.data.keyword.iot_short_notm}} 开发或部署应用程序的技术疑问，请将您的疑问发布到 [Stack Overflow](http://stackoverflow.com/search?q=watson-iot+ibm-bluemix){:new_window} 上，并使用“ibm-bluemix”和“watson-iot”标注您的疑问。
<!--Insert the appropriate dW Answers tag for your service for <service_keyword> in URL below:  -->
* 有关服务的疑问和入门指示信息，请使用 [IBM developerWorks dW Answers](https://developer.ibm.com/answers/topics/watson-iot/?smartspace=bluemix){:new_window} 论坛。请加上“watson-iot”和“bluemix”标记。

请参阅[获取帮助](https://www.{DomainName}/docs/support/index.html#getting-help)以获取有关使用论坛的更多详细信息。

有关开具 IBM 支持凭单或者有关支持级别和凭单严重性的信息，请参阅[联系支持人员](https://www.{DomainName}/docs/support/index.html#contacting-support)。
