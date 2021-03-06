---

 

copyright:

  years: 2016

 

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 使用针对 {{site.data.keyword.openwhisk_short}} 启用的 {{site.data.keyword.Bluemix_notm}} 服务
{: #openwhisk_ecosystem}
上次更新时间：2016 年 8 月 4 日
{: .last-updated}

在 {{site.data.keyword.openwhisk}} 中，通过包的目录，您能够轻松地借助多个有用的功能增强应用程序，以及访问生态系统中的外部服务。启用了 {{site.data.keyword.openwhisk_short}} 的外部服务的示例包括 Cloudant、The Weather Company、Slack 和 GitHub。
{: shortdesc}

目录在 `/whisk.system` 名称空间中作为包提供。请参阅[浏览包](./openwhisk_packages.html#openwhisk_packagedisplay)以获取有关如何使用命令行工具来浏览目录的信息。

以下主题记录了目录中的某些包。

## 使用 Cloudant 包
{: #openwhisk_catalog_cloudant}
`/whisk.system/cloudant` 包支持您使用 Cloudant 数据库。此包中包含以下操作和订阅源。

| 实体 | 类型 | 参数 | 描述 |
| --- | --- | --- | --- |
| `/whisk.system/cloudant` | 包 | {{site.data.keyword.Bluemix_notm}}ServiceName、host、username、password、dbname、includeDoc 和 overwrite | 使用 Cloudant 数据库 |
| `/whisk.system/cloudant/read` | 操作 | dbname、includeDoc 和 id | 从数据库中读取文档 |
| `/whisk.system/cloudant/write` | 操作 | dbname、overwrite 和 doc | 将文档写入数据库 |
| `/whisk.system/cloudant/changes` | 订阅源 | dbname、includeDoc、maxTriggers | 对数据库进行更改时触发触发器事件 |

以下主题将指导您设置 Cloudant 数据库，配置关联的包以及使用 `/whisk.system/cloudant` 包中的操作和订阅源。

### 在 {{site.data.keyword.Bluemix_notm}} 中设置 Cloudant 数据库
{: #openwhisk_catalog_cloudant_in}

如果是在 {{site.data.keyword.Bluemix}} 中使用 {{site.data.keyword.openwhisk_short}}，{{site.data.keyword.openwhisk_short}} 将为 {{site.data.keyword.Bluemix_notm}} Cloudant 服务实例自动创建包绑定。如果不是在 {{site.data.keyword.Bluemix_notm}} 中使用 {{site.data.keyword.openwhisk_short}} 和 Cloudant，请跳至下一步。

1. 在 {{site.data.keyword.Bluemix_notm}} [仪表板](http://console.ng.{{site.data.keyword.Bluemix_notm}}.net)中创建 Cloudant 服务实例。

  确保记住服务实例的名称以及您所在的 {{site.data.keyword.Bluemix_notm}} 组织和空间的名称。

2. 确保 {{site.data.keyword.openwhisk_short}} CLI 位于与上一步中使用的 {{site.data.keyword.Bluemix_notm}} 组织和空间对应的名称空间中。

  ```
  wsk property set --namespace my{{site.data.keyword.Bluemix_notm}}Org_my{{site.data.keyword.Bluemix_notm}}Space
  ```
  {: pre}

  或者，可以使用 `wsk property set --namespace` 从您可访问的名称空间的列表中设置名称空间。

3. 刷新名称空间中的包。刷新操作将自动为已创建的 Cloudant 服务实例创建包绑定。

  ```
wsk package refresh
  ```
  {: pre}
  ```
  created bindings:
  {{site.data.keyword.Bluemix_notm}}_testCloudant_Credentials-1
  ```
  {: screen}

  ```
  wsk package list
  ```
  {: pre}
  ```
  packages
  /my{{site.data.keyword.Bluemix_notm}}Org_my{{site.data.keyword.Bluemix_notm}}Space/{{site.data.keyword.Bluemix_notm}}_testCloudant_Credentials-1 private binding
  ```
  {: screen}

  您会看到与 {{site.data.keyword.Bluemix_notm}} Cloudant 服务实例对应的包绑定的标准名称。

4. 检查以确认先前创建的包绑定是否已使用 Cloudant {{site.data.keyword.Bluemix_notm}} 服务实例主机和凭证进行配置。

  ```
  wsk package get /my{{site.data.keyword.Bluemix_notm}}Org_my{{site.data.keyword.Bluemix_notm}}Space/{{site.data.keyword.Bluemix_notm}}_testCloudant_Credentials-1
  ```
  {: pre}
  ```
  ok: got package /my{{site.data.keyword.Bluemix_notm}}Org_my{{site.data.keyword.Bluemix_notm}}Space/{{site.data.keyword.Bluemix_notm}}_testCloudant_Credentials-1, projecting parameters
  [
      ...
      {
          "key": "username",
          "value": "cdb18832-2bbb-4df2-b7b1-{{site.data.keyword.Bluemix_notm}}"
      },
      {
          "key": "host",
          "value": "cdb18832-2bbb-4df2-b7b1-{{site.data.keyword.Bluemix_notm}}.cloudant.com"
      },
      {
          "key": "password",
          "value": "c9088667484a9ac827daf8884972737"
      }
      ...
  ]
  ```
  {: screen}

### 在 {{site.data.keyword.Bluemix_notm}} 外部设置 Cloudant 数据库
{: #openwhisk_catalog_cloudant_outside}

如果不是在 {{site.data.keyword.Bluemix_notm}} 中使用 {{site.data.keyword.openwhisk_short}}，或者如果要在 {{site.data.keyword.Bluemix_notm}} 外部设置 Cloudant 数据库，那么必须为您的 Cloudant 帐户手动创建包绑定。您需要 Cloudant 帐户主机名、用户名和密码。

1. 创建为您的 Cloudant 帐户配置的包绑定。

  ```
wsk package bind /whisk.system/cloudant myCloudant -p username 'MYUSERNAME' -p password 'MYPASSWORD' -p host 'MYCLOUDANTACCOUNT.cloudant.com'
  ```
  {: pre}

2. 验证包绑定是否存在。

  ```
  wsk package list
  ```
  {: pre}
  ```
packages
  /myNamespace/myCloudant private binding
  ```
  {: screen}


### 侦听对 Cloudant 数据库的更改
{: #openwhisk_catalog_cloudant_listen}

可以使用 `changes` 订阅源来配置服务，以在每次对 Cloudant 数据库进行更改时都触发触发器。参数如下所示：

- `dbname`：Cloudant 数据库的名称。
- `includeDoc`：如果设置为 true，那么触发的每个触发器事件都会包含修改后的 Cloudant 文档。 
- `maxTriggers`：达到此限制时，停止触发触发器。缺省值为 1000。您可以将其设置为最大值 10,000。如果您尝试设置超过 10,000，那么会拒绝该请求。

1. 使用先前创建的包绑定中的 `changes` 订阅源来创建触发器。确保将 `/myNamespace/myCloudant` 替换为您的包名。

  ```
wsk trigger create myCloudantTrigger --feed /myNamespace/myCloudant/changes --param dbname testdb --param includeDoc true
  ```
  {: pre}
  ```
ok: created trigger feed myCloudantTrigger
  ```
  {: screen}

2. 轮询激活。

  ```
wsk activation poll
  ```
  {: pre}

3. 在 Cloudant 仪表板中，修改现有文档或创建新文档。

4. 观察针对每次文档更改的 `myCloudantTrigger` 触发器的新激活。

**注**：如果无法观察到新激活，请查看有关对 Cloudant 数据库执行读写操作的后续各部分。测试以下读写步骤将帮助验证 Cloudant 凭证是否正确。

现在，可以创建规则并将其关联到操作，以对文档更新做出反应。

生成的事件的内容取决于创建触发器时 `includeDoc` 参数的值。如果参数设置为 true，那么触发的每个触发器事件都会包含修改后的 Cloudant 文档。例如，假设修改后的文档如下：

  ```
  {
    "_id": "6ca436c44074c4c2aa6a40c9a188b348",
    "_rev": "3-bc4960fc13aa368afca8c8427a1c18a8",
    "name": "Heisenberg"
  }
  ```
  {: screen}

此示例中的代码会使用对应的 `_id`、`_rev` 和 `name` 参数来生成触发器事件。事实上，触发器事件的 JSON 表示与该文档完全相同。

否则，如果 `includeDoc` 为 false，那么事件将包含以下参数：

- `id`：文档标识。
- `seq`：Cloudant 生成的序列标识。
- `changes`：对象数组，每个对象都具有 `rev` 字段，用于包含文档的修订版标识。

触发器事件的 JSON 表示如下所示：

  ```
  {
      "id": "6ca436c44074c4c2aa6a40c9a188b348",
      "seq": "2-g1AAAAL9aJyV-GJCaEuqx4-BktQkYp_dmIfC",
      "changes": [
          {
              "rev": "2-da3f80848a480379486fb4a2ad98fa16"
          }
      ]
  }
  ```


### 写入 Cloudant 数据库
{: #openwhisk_catalog_cloudant_write}

可以使用操作将文档存储在名为 `testdb` 的 Cloudant 数据库中。确保此数据库在 Cloudant 帐户中存在。

1. 使用先前创建的包绑定中的 `write` 操作来存储文档。确保将 `/myNamespace/myCloudant` 替换为您的包名。

  ```
wsk action invoke /myNamespace/myCoudant/write --blocking --result --param dbname testdb --param doc '{"_id":"heisenberg", "name":"Walter White"}'
  ```
  {: pre}
  ```
  ok: invoked /myNamespace/myCoudant/write with id 62bf696b38464fd1bcaff216a68b8287
  {
    "id": "heisenberg",
    "ok": true,
    "rev": "1-9a94fb93abc88d8863781a248f63c8c3"
  }
  ```
  {: screen}

2. 通过在 Cloudant 仪表板中浏览以查找该文档，验证该文档是否存在。

  `testdb` 数据库的仪表板 URL 类似于以下内容：`https://MYCLOUDANTACCOUNT.cloudant.com/dashboard.html#database/testdb/_all_docs?limit=100`。


### 从 Cloudant 数据库进行读取
{: #openwhisk_catalog_cloudant_read}

可以使用操作从名为 `testdb` 的 Cloudant 数据库中访存文档。确保此数据库在 Cloudant 帐户中存在。

1. 使用先前创建的包绑定中的 `read` 操作来访存文档。确保将 `/myNamespace/myCloudant` 替换为您的包名。

  ```
wsk action invoke /myNamespace/myCoudant/read --blocking --result --param dbname testdb --param id heisenberg
  ```
  {: pre}
  ```
  {
    "_id": "heisenberg",
    "_rev": "1-9a94fb93abc88d8863781a248f63c8c3"
    "name": "Walter White"
  }
  ```
  {: screen}


## 使用 Alarm 包
{: #openwhisk_catalog_alarm}

`/whisk.system/alarms` 包可用于按指定的频率触发触发器。这对于设置重复发生的作业或任务（例如，每小时调用一次系统备份操作）来说非常有用。

此包中包含以下订阅源。

| 实体 | 类型 | 参数 | 描述 |
| --- | --- | --- | --- |
| `/whisk.system/alarms` | 包 | - | 警报和定期实用程序 |
| `/whisk.system/alarms/alarm` | 订阅源 | cron、trigger_payload 和 maxTriggers | 定期触发触发器事件 |


### 定期触发触发器事件
{: #openwhisk_catalog_alarm_fire}

`/whisk.system/alarms/alarm` 订阅源将警报服务配置为按指定的频率触发触发器事件。参数如下所示：

- `cron`：字符串，基于 UNIX crontab 语法，用于指示何时触发触发器，时间采用全球标准时间 (UTC)。此字符串是 6 个字段组成的序列，字段之间用空格分隔：`X X X X X X `。有关使用 cron 语法的更多详细信息，请参阅 https://github.com/ncb000gt/node-cron。以下是该字符串所指示的一些频率示例：

  - `* * * * * *`：每一秒。
  - `0 * * * * *`：每一分钟的顶部。
  - `* 0 * * * *`：每一小时的顶部。
  - `0 0 9 8 * *`：每一个月第八天的上午 9:00:00 (UTC)

- `trigger_payload`：此参数的值成为每次触发器触发时触发器的内容。

- `maxTriggers`：达到此限制时，停止触发触发器。缺省值为 1000。您可以将其设置为最大值 10,000。如果您尝试设置超过 10,000，那么会拒绝该请求。

以下是通过触发器事件中的 `name` 和 `place` 值创建每 20 秒将触发一次的触发器的示例。

  ```
  wsk trigger create periodic --feed /whisk.system/alarms/alarm --param cron '*/20 * * * * *' --param trigger_payload '{"name":"Odin","place":"Asgard"}'
  ```
  {: pre}

每次生成的事件将包含 `trigger_payload` 值中指定的属性作为参数。在本例中，每个触发器事件将包含参数 `name=Odin` 和 `place=Asgard`。


## 使用 Weather 包
{: #openwhisk_catalog_weather}

通过 `/whisk.system/weather` 包，可以方便地调用 Weather Company Data for IBM Bluemix API。

此包中包含以下操作。

| 实体 | 类型 | 参数 | 描述 |
| --- | --- | --- | --- |
| `/whisk.system/weather` | 包 | username 和 password | 来自 Weather Company Data for IBM Bluemix API 的服务  |
| `/whisk.system/weather/forecast` | 操作 | latitude、longitude、timePeriod | 指定时间段的预报|

建议使用 `username` 和 `password` 值创建包绑定。这样就无需在每次调用包中的操作时指定这些凭证。

### 获取某个位置的天气预报
{: #openwhisk_catalog_weather_forecast}

`/whisk.system/weather/forecast` 操作通过从 The Weather Company 调用 API，返回某个位置的天气预报。参数如下所示：

- `username`：Weather Company Data for IBM Bluemix 的用户名，此用户名有权调用预测 API。
- `password`：Weather Company Data for IBM Bluemix 的密码，此密码有权调用预测 API。
- `latitude`：位置的纬度坐标。
- `longitude`：位置的经度坐标。
- `timeperiod`：预报的时间段。有效选项为 '10day' -（缺省值）返回每日 10 天预报，'24hour' - 返回每小时 2 天预报，'current' - 返回当前天气条件，'timeseries' - 返回当前观察和长达过去 24 小时（从当前日期和时间）的观察。 


以下是创建包绑定并获取 10 天天气预报的示例。

1. 使用 API 密钥创建包绑定。

  ```
wsk package bind /whisk.system/weather myWeather --param apiKey 'MY_WEATHER_API'
  ```
  {: pre}

2. 调用包绑定中的 `forecast` 操作来获取天气预报。

  ```
wsk action invoke myWeather/forecast --blocking --result --param latitude '43.7' --param longitude '-79.4'
  ```
  {: pre}

  ```
  {
      "forecasts": [
          {
              "dow": "Wednesday",
              "max_temp": -1,
              "min_temp": -16,
              "narrative": "Chance of a few snow showers. Highs -2 to 0C and lows -17 to -15C.",
              ...
          },
          {
              "class": "fod_long_range_daily",
              "dow": "Thursday",
              "max_temp": -4,
              "min_temp": -8,
              "narrative": "Mostly sunny. Highs -5 to -3C and lows -9 to -7C.",
              ...
          },
          ...
      ],
  }
  ```
  {: screen}


## 使用 Watson 包
{: #openwhisk_catalog_watson}

通过 `/whisk.system/watson` 包，可以方便地调用各种 Watson API。

此包中包含以下操作。

| 实体 | 类型 | 参数 | 描述 |
| --- | --- | --- | --- |
| `/whisk.system/watson` | 包 | username 和 password | Watson Analytics API 的操作 |
| `/whisk.system/watson/translate` | 操作 | translateFrom、translateTo、translateParam、username 和 password | 翻译文本 |
| `/whisk.system/watson/languageId` | 操作 | payload、username 和 password | 识别语言 |
| `/whisk.system/watson/speechToText` | 操作 | payload、content_type、encoding、username、password、continuous、inactivity_timeout、interim_results、keywords、keywords_threshold、max_alternatives、model、timestamps、watson-token、word_alternatives_threshold、word_confidence、X-Watson-Learning-Opt-Out | 将音频转换为文本 |
| `/whisk.system/watson/textToSpeech` | 操作 | payload、voice、accept、encoding、username、password | 将文本转换为音频 |

建议使用 `username` 和 `password` 值创建包绑定。这样就无需在每次调用包中的操作时指定这些凭证。



### 翻译文本
{: #openwhisk_catalog_watson_translate}

`/whisk.system/watson/translate` 操作将文本从一种语言翻译为另一种语言。参数如下所示：

- `username`：Watson API 用户名。
- `password`：Watson API 密码。
- `translateParam`：指示要翻译文本的输入参数。例如，如果 `translateParam=payload`，那么将翻译传递给该操作的 `payload` 参数的值。
- `translateFrom`：源语言的两位数代码。
- `translateTo`：目标语言的两位数代码。

下面是创建包绑定并翻译某些文本的示例。

1. 使用 Watson 凭证创建包绑定。

  ```
wsk package bind /whisk.system/watson myWatson --param username 'MY_WATSON_USERNAME' --param password 'MY_WATSON_PASSWORD'
  ```
  {: pre}

2. 调用包绑定中的 `translate` 操作，以将某些文本从英语翻译为法语。

  ```
wsk action invoke myWatson/translate --blocking --result --param payload 'Blue skies ahead' --param translateParam 'payload' --param translateFrom 'en' --param translateTo 'fr'
  ```
  {: pre}

  ```
  {
      "payload": "Ciel bleu a venir"
  }
  ```
  {: screen}


### 识别某些文本的语言
{: #openwhisk_catalog_watson_identifylang}

`/whisk.system/watson/languageId` 操作可识别某些文本的语言。参数如下所示：

- `username`：Watson API 用户名。
- `password`：Watson API 密码。
- `payload`：要识别的文本。

以下是创建包绑定并识别某些文本的语言的示例。

1. 使用 Watson 凭证创建包绑定。

  ```
wsk package bind /whisk.system/watson myWatson -p username 'MY_WATSON_USERNAME' -p password 'MY_WATSON_PASSWORD'
  ```
  {: pre}

2. 调用包绑定中的 `languageId` 操作来识别语言。

  ```
wsk action invoke myWatson/languageId --blocking --result --param payload 'Ciel bleu a venir'
  ```
  {: pre}
  ```
  {
    "payload": "Ciel bleu a venir",
    "language": "fr",
    "confidence": 0.710906
  }
  ```
  {: screen}


### 将某些文本转换为语音
{: #openwhisk_catalog_watson_texttospeech}

`/whisk.system/watson/textToSpeech` 操作可将某些文本转换为音频语音。参数如下所示：

- `username`：Watson API 用户名。
- `password`：Watson API 密码。
- `payload`：要转换为语音的文本。
- `voice`：讲话者的声音。
- `accept`：语音文件的格式。
- `encoding`：语音二进制数据的编码。

以下是创建包绑定并将某些文本转换为语音的示例。

1. 使用 Watson 凭证创建包绑定。

  ```
wsk package bind /whisk.system/watson myWatson -p username 'MY_WATSON_USERNAME' -p password 'MY_WATSON_PASSWORD'
  ```
  {: pre}

2. 调用包绑定中的 `textToSpeech` 操作来转换文本。

  ```
wsk action invoke myWatson/textToSpeech --blocking --result --param payload 'Hey.' --param voice 'en-US_MichaelVoice' --param accept 'audio/wav' --param encoding 'base64'
  ```
  {: pre}
  ```
  {
        "payload": "<base64 encoding of a .wav file>"
  }
  ```
  {: screen}


### 将语音转换为文本
{: #openwhisk_catalog_watson_speechtotext}

`/whisk.system/watson/speechToText` 操作可将音频语音转换为文本。参数如下所示：

- `username`：Watson API 用户名。
- `password`：Watson API 密码。
- `payload`：要转换为文本的已编码语音二进制数据。
- `content_type`：音频的 MIME 类型。
- `encoding`：语音二进制数据的编码。
- `continuous`：指示是否返回代表长时间停顿所分隔的连续短语的多个最终结果。
- `inactivity_timeout`：如果在所提交的音频中只检测到沉默，那么在该时间（秒）之后，会关闭连接。
- `interim_results`：指示服务是否返回中间结果。
- `keywords`：要在音频中抓住的关键字列表。
- `keywords_threshold`：抓住关键字的下限置信度值。
- `max_alternatives`：要返回的替代脚本的最大数目。
- `model`：要用于辨识请求的模型的标识。
- `timestamps`：指示是否针对每一个字返回时间校准。
- `watson-token`：为服务提供认证令牌，作为提供服务凭证的替代方法。
- `word_alternatives_threshold`：将假设识别为可能的替代文字的下限置信度值。
- `word_confidence`：指示是否针对每一个字返回 0 到 1 范围内的置信度测量。
- `X-Watson-Learning-Opt-Out`：指示是否针对调用选择退出数据收集。
 
以下是创建包绑定并将语音转换为文本的示例。

1. 使用 Watson 凭证创建包绑定。

  ```
wsk package bind /whisk.system/watson myWatson -p username 'MY_WATSON_USERNAME' -p password 'MY_WATSON_PASSWORD'
  ```
  {: pre}

2. 调用包绑定中的 `speechToText` 操作来转换已编码的音频。

  ```
  wsk action invoke myWatson/speechToText --blocking --result --param payload <base64 encoding of a .wav file> --param content_type 'audio/wav' --param encoding 'base64'
  ```
  {: pre}
  ```
  {
        "data": "Hello Watson"
  }
  ```
  {: screen}
  
 
## 使用 Slack 包
{: #openwhisk_catalog_slack}

通过 `/whisk.system/slack` 包，可以方便地使用 [Slack API](https://api.slack.com/)。

此包中包含以下操作：

| 实体 | 类型 | 参数 | 描述 |
| --- | --- | --- | --- |
| `/whisk.system/slack` | 包 | url、channel 和 username | 与 Slack API 进行交互 |
| `/whisk.system/slack/post` | 操作 | text、url、channel 和 username | 将消息发布到 Slack 通道 |

建议使用 `username`、`url` 和 `channel` 值创建包绑定。通过绑定，就无需在每次调用包中的操作时指定这些值。

### 将消息发布到 Slack 通道
{: #openwhisk_catalog_slack_post}

`/whisk.system/slack/post` 操作可将消息发布到指定的 Slack 通道。参数如下所示：

- `url`：Slack Webhook URL。
- `channel`：要将消息发布到的 Slack 通道。
- `username`：发布消息的用户的名称。
- `text`：要发布的消息。

下面是配置 Slack、创建包绑定并将消息发布到通道的示例。

1. 为您的团队配置 Slack [入局 Webhook](https://api.slack.com/incoming-webhooks)。

  配置 Slack 后，您会获得类似于以下内容的 Webhook URL：`https://hooks.slack.com/services/aaaaaaaaa/bbbbbbbbb/cccccccccccccccccccccccc`。下一步中将需要此 URL。

2. 使用 Slack 凭证、要发布到的通道和执行发布的用户名来创建包绑定。

  ```
wsk package bind /whisk.system/slack mySlack --param url 'https://hooks.slack.com/services/...' --param username 'Bob' --param channel '#MySlackChannel'
  ```
  {: pre}

3. 调用包绑定中的 `post` 操作，以将消息发布到 Slack 通道。

  ```
wsk action invoke mySlack/post --blocking --result --param text 'Hello from OpenWhisk!'
  ```
  {: pre}


## 使用 GitHub 包
{: #openwhisk_catalog_github}

通过 `/whisk.system/github` 包，可以方便地使用 [GitHub API](https://developer.github.com/)。

此包中包含以下订阅源：

| 实体 | 类型 | 参数 | 描述 |
| --- | --- | --- | --- |
| `/whisk.system/github` | 包 | username、repository 和 accessToken | 与 GitHub API 进行交互 |
| `/whisk.system/github/webhook` | 订阅源 | events、username、repository 和 accessToken | 对 GitHub 活动触发触发器事件 |

建议使用 `username`、`repository` 和 `accessToken` 值创建包绑定。通过绑定，就无需在每次使用包中的订阅源时指定这些值。

### 使用 GitHub 活动触发触发器事件
{: #openwhisk_catalog_github_fire}

`/whisk.system/github/webhook` 订阅源将服务配置为当指定的 GitHub 存储库中发生活动时触发触发器。参数如下所示：

- `username`：GitHub 存储库的用户名。
- `repository`：GitHub 存储库。
- `accessToken`：您的 GitHub 个人访问令牌。[创建令牌](https://github.com/settings/tokens)时，请确保选择 repo:status 和 public_repo 作用域。此外，请确保还没有为存储库定义任何 Webhook。
- `events`：相关的 [GitHub 事件类型](https://developer.github.com/v3/activity/events/types/)。

下面是创建触发器的示例，此触发器将在每次 GitHub 存储库中发生新落实时触发。

1. 生成 GitHub [个人访问令牌](https://github.com/settings/tokens)。

  下一步中将使用此访问令牌。

2. 使用访问令牌创建为您的 GitHub 存储库配置的包绑定。

  ```
wsk package bind /whisk.system/github myGit --param username myGitUser --param repository myGitRepo --param accessToken aaaaa1111a1a1a1a1a111111aaaaaa1111aa1a1a
  ```
  {: pre}

3. 使用 `myGit/webhook` 订阅源为 GitHub `push` 事件类型创建触发器。

  ```
wsk trigger create myGitTrigger --feed myGit/webhook --param events push
  ```
  {: pre}

使用 `git push` 对 Github 存储库作出的承诺将导致触发器由 Webhook 触发。如果存在与触发器匹配的规则，那么将会调用相关联的操作。
该操作会接收 GitHub Webhook 有效内容作为输入参数。每一个 GitHub Webhook 事件都具有相似的 JSON 模式，但是却是事件类型所确定的唯一有效内容对象。
有关更多有效内容的内容信息，请参阅 [GitHub 事件和有效内容](https://developer.github.com/v3/activity/events/types/) API 文档。


## 使用 Push 包
{: #openwhisk_catalog_pushnotifications}

`/whisk.system/pushnotifications` 包允许您使用推送服务。 

此包中包含以下操作和订阅源：

| 实体 | 类型 | 参数 | 描述 |
| --- | --- | --- | --- |
| `/whisk.system/pushnotifications` | 包 | appId、appSecret  | 使用推送服务 |
| `/whisk.system/pushnotifications/sendMessage` | 操作 | text、url、deviceIds、platforms、tagNames、apnsBadge、apnsCategory、apnsActionKeyTitle、apnsSound、apnsPayload、apnsType、gcmCollapseKey、gcmDelayWhileIdle、gcmPayload、gcmPriority、gcmSound、gcmTimeToLive | 将推送通知发送到一个或多个指定设备 |
| `/whisk.system/pushnotifications/webhook` | 订阅源 | 事件 | 在推送服务的设备活动（设备注册、取消注册、预订或取消预订）上触发触发器事件 |
建议使用 `appId` 和 `appSecret` 值创建包绑定。这样就无需在每次调用包中的操作时指定这些凭证。

### 创建 Push 包绑定
{: #openwhisk_catalog_pushnotifications_create}

创建 Push Notification 包绑定时，必须指定以下参数：

-  `appId`：Bluemix 应用程序 GUID。
-  `appSecret`：Bluemix 推送通知服务 appSecret。

下面是创建包绑定的示例。

1. 在 [Bluemix 仪表板](http://console.ng.bluemix.net)中创建 Bluemix 应用程序。

2. 初始化推送通知服务，并将该服务绑定到 Bluemix 应用程序

3. 配置 [Push Notification 应用程序](https://console.ng.bluemix.net/docs/services/mobilepush/index.html)。

  确保记住您所创建的 Bluemix 应用程序的 `App GUID` 和 `App Secret`。


4. 创建与 `/whisk.system/pushnotifications` 的包绑定。

  ```
  wsk package bind /whisk.system/pushnotifications myPush -p appId "myAppID" -p appSecret "myAppSecret"
  ```
  {: pre}

5. 验证包绑定是否存在。

  ```
  wsk package list
  ```
  {: pre}

  ```
  packages
  /myNamespace/myPush private binding
  ```
  {: screen}

### 发送推送通知
{: #openwhisk_catalog_pushnotifications_send}

`/whisk.system/pushnotifications/sendMessage` 操作会将推送通知发送到已注册的设备。参数如下所示：
- `text`：要向用户显示的通知消息。例如：`-p text "Hi ,OpenWhisk send a notification"`。
- `url`：可与警报一起发送的可选 URL。例如：`-p url "https:\\www.w3.ibm.com"`。
- `gcmPayload`：作为通知消息一部分发送的定制 JSON 有效内容。例如：`-p gcmPayload "{"hi":"hello"}"`
- `gcmSound`：通知到达设备时，要尝试播放的声音文件（在设备上）。
- `gcmCollapseKey`：此参数可识别一组消息
- `gcmDelayWhileIdle`：当此参数设置为 true 时，表示在设备变为活动之前，不会发送消息。
- `gcmPriority`：设置消息的优先级。
- `gcmTimeToLive`：此参数指定当设备脱机时，会在 GCM 存储器中保留消息的时间长度（秒）。
- `apnsBadge`：显示为应用程序图标的角标的数字。
- `apnsCategory`：要用于交互式推送通知的类别标识。
- `apnsIosActionKey`：Action 键的标题。
- `apnsPayload`：作为通知消息一部分发送的定制 JSON 有效内容。
- `apnsType`：[“DEFAULT”、“MIXED”、“SILENT”]。
- `apnsSound`：应用程序捆绑软件中声音文件的名称。此文件的声音播放为警报。

以下是 pushnotification 包中发送推送通知的示例。

1. 使用先前创建的包绑定中的 `sendMessage` 操作来发送推送通知。确保将 `/myNamespace/myPush` 替换为您的包名。

  ```
  wsk action invoke /myNamespace/myPush/sendMessage --blocking --result  -p url https://example.com -p text "this is my message"  -p sound soundFileName -p deviceIds '["T1","T2"]'
  ```
  {: pre}

  ```
  {
  "result": {
          "pushResponse": "{"messageId":"11111H","message":{"message":{"alert":"this is my message","url":"http.google.com"},"settings":{"apns":{"sound":"default"},"gcm":{"sound":"default"},"target":{"deviceIds":["T1","T2"]}}}"
  },
      "status": "success",
      "success": true
  }
  ```
  {: screen}

### 在 Push 活动上触发触发器事件
{: #openwhisk_catalog_pushnotifications_fire}

`/whisk.system/pushnotifications/webhook` 可配置 Push 服务，当在指定的应用程序中存在设备活动（如设备注册/取消注册或预订/取消预订）时，触发触发器。

参数如下所示：

- `appId：`Bluemix 应用程序 GUID。
- `appSecret：`Bluemix 推送通知服务 appSecret。
- `events：`受支持的事件为 `onDeviceRegister`、`onDeviceUnregister`、`onDeviceUpdate`、`onSubscribe`、`onUnsubscribe`。要在发生所有事件时都获得通知，请使用通配符 `*`。

以下是创建触发器的示例，此触发器将在每次向 Push Notifications 服务应用程序注册新设备时触发。

1. 使用 appId 和 appSecret，创建针对 Push Notifications 服务配置的包绑定。

  ```
  wsk package bind /whisk.system/pushnotifications myNewDeviceFeed --param appID myapp --param appSecret myAppSecret --param events onDeviceRegister
  ```
  {: pre}

2. 使用 `myPush/webhook` 订阅源为 Push Notifications 服务 `onDeviceRegister` 事件类型创建触发器。

  ```
  wsk trigger create myPushTrigger --feed myPush/webhook --param events onDeviceRegister
  ```
  {: pre}
