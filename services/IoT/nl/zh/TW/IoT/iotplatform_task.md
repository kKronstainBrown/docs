---

copyright:
  years: 2016

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 連接裝置
{: #iotplatform_task}
前次更新：2016 年 9 月 13 日
{: .last-updated}

您必須先將 IoT 裝置連接至 {{site.data.keyword.iot_full}}，才能開始從 IoT 裝置接收資料。若要將裝置連接至 {{site.data.keyword.iot_short_notm}}，需要向 {{site.data.keyword.iot_short_notm}} 登錄裝置，然後使用登錄資訊，將裝置配置為連接至 {{site.data.keyword.iot_short_notm}}。
{:shortdesc}

## 開始之前
{: #byb}
 
在開始連線程序之前，您必須確定裝置符合與 {{site.data.keyword.iot_short_notm}} 通訊的下列需求：

- 您的裝置必須能夠透過傳送 [MQTT 格式](reference/mqtt/index.html)的裝置訊息來通訊。
- 裝置訊息必須符合 {{site.data.keyword.iot_short_notm}} [訊息有效負載](reference/mqtt/index.html#message-payload)需求。

請完成下列步驟，以將裝置連接至 {{site.data.keyword.iot_short_notm}}。

## 步驟 1：向 {{site.data.keyword.iot_short_notm}} 登錄您的裝置  
{: #iotplatform_subtask1}

若要登錄裝置，需要將裝置分類成裝置類型、為裝置命名，並提供裝置資訊。然後，您可以提供連線記號，或是接受 {{site.data.keyword.iot_short_notm}} 產生的記號。

您可以從 {{site.data.keyword.iot_short_notm}} 儀表板一次新增一個裝置，或者可以使用 [{{site.data.keyword.iot_short_notm}} API](https://docs.internetofthings.ibmcloud.com/swagger/v0002.html#!/Bulk_Operations/post_bulk_devices_add)，一次新增一個以上裝置。

若要從 {{site.data.keyword.iot_short_notm}} 儀表板新增裝置，請執行下列動作：

1. 按一下 {{site.data.keyword.Bluemix}} 儀表板中的 {{site.data.keyword.iot_short_notm}} 服務磚，以開啟服務儀表板，或是使用下列 URL 直接前往儀表板：

 `https://org_id.internetofthings.ibmcloud.com/dashboard/#/overview `

    其中 *org_id* 是 {{site.data.keyword.Bluemix}} 組織的 ID。

2. 在服務頁面上，按一下**啟動儀表板**，開始管理您的 {{site.data.keyword.iot_short_notm}} 組織。

3. 在「概觀」儀表板中，從功能表窗格中選取**裝置**，然後按一下**新增裝置**。
5. 選取或建立您要新增之裝置的裝置類型。連接至 {{site.data.keyword.iot_short_notm}} 的每一個裝置都必須與裝置類型相關聯。裝置類型是一群共用相同性質的裝置。當您將第一個裝置新增至 {{site.data.keyword.iot_short_notm}} 組織時，**裝置類型**功能表中並沒有可用的裝置類型。您必須先建立一個裝置類型：
 1. 按一下**建立裝置類型**。
 2. 輸入裝置類型的名稱（例如 `my_device_type`）和說明。
 3. 選用項目：輸入裝置類型屬性和 meta 資料。
 **提示：**您可以稍後再新增及編輯屬性和 meta 資料。
 4. 按一下**建立**，以新增裝置類型。
10. 按**下一步**，開始新增所選取裝置類型的裝置。
11. 輸入裝置 ID。**提示：**若為連接網路的裝置，這個值可以是（例如）沒有任何分隔冒號的裝置 MAC 位址。裝置 ID 可用來識別 {{site.data.keyword.iot_short_notm}} 儀表板中的裝置，它也是將裝置連接至 {{site.data.keyword.iot_short_notm}} 的必要參數。
12. 選用項目：按一下**其他欄位**，以新增裝置資訊，例如序號、製造商、機型等等。
 **提示：**您可以稍後再新增及編輯此資訊。
12. 選用項目：輸入裝置 JSON meta 資料。
 **提示：**您可以稍後再新增及編輯裝置 meta 資料。
13. 按**下一步**，以完成新增裝置。
14. 驗證摘要資訊正確無誤，然後按一下**新增**，以新增連線。**提示：**您可以選擇接受自動產生的鑑別記號，或是自行提供鑑別記號。如果選擇建立自己的記號，請確定其長度為 8 到 36 個字元、包含大小寫混合的字母、數字，以及連字號、底線或句點。記號不得包含重複的字元順序、字典單字、使用者名稱或其他預先定義的順序。
15. 在裝置資訊頁面中，複製並儲存下列裝置資訊：  
 - 組織 ID，例如 `tubo8x`
 - 裝置類型，例如 `my_device_type`
 - 裝置 ID，例如 `my_first_device`
 - 鑑別方法，例如 `token`
 - 鑑別記號，例如 `PtBVriRqIg4uh)_-Kl`。
  **提示：**您需要有「組織 ID」、「鑑別記號」、「裝置類型」和「裝置 ID」，才能配置您的裝置來連接至 {{site.data.keyword.iot_short_notm}}。  

恭喜，您已登錄裝置。現在您可以配置裝置來連接至 {{site.data.keyword.iot_short_notm}}

## 步驟 2：將您的裝置連接至 {{site.data.keyword.iot_short_notm}}
{: #iotplatform_subtask2}

向 {{site.data.keyword.iot_short_notm}} 登錄裝置之後，您可以使用登錄資訊來連接裝置，並開始開始接收裝置資料。

{{site.data.keyword.iot_short_notm}} 支援許多裝置類型。連接裝置的基本程序通常包括下列步驟：
- 設定裝置來進行 MQTT 傳訊，並使用「組織 ID」、「鑑別記號」、「裝置類型」和「裝置 ID」來鑑別。  
- 使用 MQTT 通訊協定將裝置訊息傳送至 {{site.data.keyword.iot_short_notm}} 組織。

**提示：**常用的裝置有許多連線秘訣。若要取得秘訣清單，請參閱 IBM.com 上的[裝置連線秘訣](https://developer.ibm.com/recipes/?post_type=tutorials&s=IoT)。

連接裝置時，需要下列資訊：
- URL：*org_id*.messaging.internetofthings.ibmcloud.com
其中 *org_id* 是 {{site.data.keyword.iot_short_notm}} 組織的 ID。
- 埠：
 - 1883
 - 8883（已加密）
 - 443 (websockets)
- 裝置 ID：d:*org_id*:*device_type*:*device_id*
這個參數組合可唯一地識別您的裝置。
- 使用者名稱：use-token-auth
此值指出您是使用記號授權。
- 密碼：*鑑別記號*
此值是您定義的唯一記號，或是您在登錄時指派給裝置的唯一記號。
- 事件主題格式：iot-2/evt/*event_id*/fmt/*format_string*
其中 *event_id* 指定顯示在 {{site.data.keyword.iot_short_notm}} 中的事件名稱，而 *format_string* 是事件的格式，例如 JSON。
- 訊息格式：JSON
{{site.data.keyword.iot_short_notm}} 支援數種格式，例如 JSON 和文字。

如需連接裝置的相關資訊，請參閱技術說明文件中的[裝置的 MQTT 連線功能](devices/mqtt.html)。
API 文件的[連線功能](https://docs.internetofthings.ibmcloud.com/swagger/v0002.html#!/Connectivity/post_device_types_deviceType_devices_deviceId_events_eventName)區段也包含必要的資訊。
