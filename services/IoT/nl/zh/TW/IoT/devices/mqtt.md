---

copyright:
  years: 2015, 2016

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# 裝置的 MQTT 連線功能
{: #mqtt}
前次更新：2016 年 9 月 9 日
{: .last-updated}

MQTT 是裝置及應用程式用來與 {{site.data.keyword.iot_full}} 通訊的主要通訊協定。提供用戶端程式庫、資訊及範例，旨在協助您連接及整合裝置與 {{site.data.keyword.iot_short_notm}}。
{:shortdesc}

## 用戶端連線
{: #client_connections}

如需用戶端安全以及如何將 MQTT 用戶端連接至 {{site.data.keyword.iot_short_notm}} 中裝置的相關資訊，請參閱[將應用程式、裝置及閘道連接至 {{site.data.keyword.iot_short_notm}}](../reference/security/connect_devices_apps_gw.html)。


## 將裝置連接至快速入門服務
{: #connecting_devices}

「快速入門」服務是最快速的服務水準。它不提供任何接收確認，也不支援大於零的 MQTT 服務品質 (QoS) 水準。當您連接至「快速入門」服務時，不需要鑑別或登錄，而且 `orgId` 必須設為 `quickstart`。

如果您是撰寫要與「快速入門」搭配使用的裝置程式碼，請注意，「快速入門」模式中不支援下列 {{site.data.keyword.iot_short_notm}} 服務特性：

-  訂閱指令
-  裝置管理通訊協定
-  全新或可延續階段作業

**重要事項：**從裝置傳送的訊息，若傳送速率大於每秒一個，可能遭到捨棄。


## MQTT 鑑別
{: #mqtt_authentication}

對於閘道和裝置，{{site.data.keyword.iot_short_notm}} 會使用 MQTT 記號型鑑別。

若要啟用 MQTT 鑑別，請在建立 MQTT 連線時，提交使用者名稱和密碼。

### 使用者名稱

所有裝置的使用者名稱值皆相同：`use-token-auth`。這個值會導致 {{site.data.keyword.iot_short_notm}} 使用裝置的鑑別記號（指定作為密碼）。

### 密碼

每一個裝置的密碼都是向 {{site.data.keyword.iot_short_notm}} 登錄裝置時所產生的唯一鑑別記號。

## 發佈事件
{: #publishing_events}

裝置會以下列格式發佈至事件主題：

<pre class="pre">iot-2/evt/<var class="keyword varname">event_id</var>/fmt/<var class="keyword varname">format_string</var></pre>
{: codeblock}

其中

-  **event_id** 是事件的 ID，例如，`status`。事件 ID 可以是 MQTT 中任何有效的字串。如果未使用萬用字元，則訂閱者應用程式必須在其訂閱主題中使用此字串，才能接收在其主題上發佈的事件。
-  **format_string** 是事件有效負載的格式，例如，`json`。格式可以是 MQTT 中任何有效的字串。如果未使用萬用字元，則訂閱者應用程式必須在其訂閱主題中使用此字串，才能接收在其主題上發佈的事件。

**重要事項：**訊息有效負載上限為 131072 個位元組。大於此限制的訊息都會被拒絕。


## 訂閱指令
{: #subscribing_to_commands}

裝置可用下列格式訂閱指令主題：

<pre class="pre">iot-2/cmd/<var class="keyword varname">command_id</var>/fmt/<var class="keyword varname">format_string</var></pre>
{: codeblock}

其中
 - **command_id** 是指令的 ID，例如，`update`。指令 ID 可以是 MQTT 通訊協定中任何有效的字串。如果未使用萬用字元，則裝置必須在其訂閱主題中使用此字串，才能接收在其主題上發佈的指令。
 - **format_string** 是指令有效負載的格式，例如，`json`。格式可以是 MQTT 通訊協定中任何有效的字串。如果未使用萬用字元，則裝置必須在其訂閱主題中使用此字串，才能接收在其主題上發佈的指令。

裝置無法從其他裝置訂閱事件。裝置只會接收發佈至其專屬裝置的指令。

## 受管理裝置
{: #managed-devices}

對裝置生命週期管理的支援是選用性的。「裝置管理通訊協定」使用您的裝置已用於事件和指令控制的相同 MQTT 連線。

### 服務品質水準及全新階段作業

受管理裝置可以發佈服務品質 (QoS) 水準為 0 或 1 的訊息。來自該裝置的訊息不得為保留的訊息。

QoS=0 的訊息可以捨棄，而且在傳訊伺服器重新啟動之後，不會持續保存。QoS=1 的訊息可以置入佇列，而且在傳訊伺服器重新啟動之後，會持續保存。訂閱的延續性會決定是否將要求置入佇列。用來訂閱之連線的 `cleansession` 參數會決定訂閱的延續性。  

{{site.data.keyword.iot_short_notm}} 會發佈 QoS 水準為 1 的要求，以支援訊息的佇列作業。若要將受管理裝置未連線時所傳送的訊息置入佇列，請將 `cleansession` 參數設為 false，以將裝置配置為不使用全新階段作業。

**警告：**
  如果您的受管理裝置使用可延續訂閱，若裝置未於要求逾時之前重新連接至服務，則會將裝置離線時傳送至其中的任何裝置管理指令報告為失敗作業。不過，當裝置重新連接時，裝置即會處理那些要求。可延續訂閱是由 `cleansession=false` 參數指定。

### 主題

受管理裝置必須訂閱下列主題，才能處理來自 {{site.data.keyword.iot_short_notm}} 服務的要求和回應：

```
iotdm-1/#
```


受管理裝置會發佈至正在執行之管理要求類型特有的主題：

- 受管理裝置會發佈 `iotdevice-1/response` 上的裝置管理回應。
- 如需受管理裝置可以發佈至其中的其他主題，請參閱[裝置管理通訊協定](device_mgmt/index.html)及[裝置管理要求](device_mgmt/requests.html)。



### 訊息格式

所有訊息都會以 JSON 格式傳送。

**要求**  
要求會格式化，如下列程式碼範例所示：

<pre class="pre">{  "d": {...}, "<var class="keyword varname">reqId</var>": "b53eb43e-401c-453c-b8f5-94b73290c056" }</pre>
{: codeblock}

其中：

 - **d** 負載與要求相關的任何資料。
 - **reqId** 是要求的 ID，必須複製到回應中。如果回應不是必要項目，則可以省略 *reqId* 欄位。

**回應**  
回應會格式化，如下列程式碼範例所示：
```
        {
            "rc": 0,
            "message": "success",
            "d": {...},
            "reqId": "b53eb43e-401c-453c-b8f5-94b73290c056"
        }
```
其中：  
 - **rc** 是原始要求的結果碼。
 - **message** 是含有回應碼文字說明的選用性元素。
 - **d** 是隨附於回應的選用性資料元素。
 - **reqId** 是原始要求的要求 ID，用來產生回應與要求的關聯。裝置必須確保所有要求 ID 都是唯一。{{site.data.keyword.iot_short_notm}} 要求的回應必須包括正確的 **reqId** 值。

如需特定要求及回應訊息的相關資訊，請參閱[裝置管理通訊協定](device_mgmt/index.html)及[裝置管理要求](device_mgmt/requests.html)。
