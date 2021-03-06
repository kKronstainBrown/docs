---

copyright:
  years: 2015, 2016

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 外部服務整合
{: #ref-index}
前次更新：2016 年 9 月 13 日
{: .last-updated}

外部服務整合可讓您從協力廠商或 {{site.date.keyword.iot_full}} 組織中的外部服務存取資料與作業。

## Jasper
{: #jasper}

Jasper 是 SIM 裝置的系統管理和管理平台。Jasper 整合在 {{site.data.keyword.iot_short_notm}} 儀表板中，讓您能夠透過 {{site.data.keyword.iot_short_notm}} 組織儀表板來管理 Jasper 裝置。

### 支援的 Jasper 作業

我們平台所提供的內建 Jasper 整合為下列 Jasper 作業提供支援：

- 檢視整體 Jasper 資料
  - 顯示：狀態、費率方案、月初至今的資料使用情形、月初至今的 SMS 使用情形、月初至今的語音使用情形、超額限制、新增日期和修改日期。
- 變更 SIM 啟動狀態。
  - 從下列項目中選取：庫存、啟動就緒、已啟動、已關閉、已淘汰。
- 檢視 SIM 使用情形
  - 顯示：週期開始日期、可入帳資料和資料總計、可入帳 SMS 和 SMS 總計、可入帳語音和語音總計。
  - 週期開始日期可使用 YYYY-MM-DD 格式來設定。
- 傳送 SMS 至 SIM
- 變更費率方案

完成下列配置步驟之後，您可以在 Jasper 連接裝置的裝置往下探查中，存取支援的作業：


### Jasper 的配置

為了能夠將 Jasper 服務連接至 {{site.data.keyword.iot_short_notm}} 組織，必須先完成兩個配置階段。首先，{{site.data.keyword.iot_short_notm}} 必須連接至 Jasper 裝置，然後必須配置 {{site.data.keyword.iot_short_notm}} 裝置。


1. 啟用 Jasper 延伸規格。若要讓 Jasper 與 {{site.data.keyword.iot_short_notm}} 組織整合，請完成下列步驟：
  1. 從 {{site.data.keyword.iot_short_notm}} 儀表板選取**延伸規格**。
  2. 在**延伸規格**頁面中，按一下**新增延伸規格**。
  3. 按一下 AT&AMP;T 旁的**新增**。
  4. 輸入 AT&AMP;T 使用者名稱、密碼、存取鍵及網域 ID。
  5. 按一下**完成**。

2. 配置您的裝置
您可以配置同時連接 {{site.data.keyword.iot_short_notm}} 組織和 Jasper 帳戶的裝置，以在 {{site.data.keyword.iot_short_notm}} 儀表板中顯示 Jasper 的資料。
**重要事項：**在「新增裝置」程序中無法套用 Jasper 配置，只有先前已連接的裝置可以用 Jasper 來配置。若要配置 Jasper 連接的裝置，請完成下列步驟：
 1. 在 {{site.data.keyword.iot_short_notm}} 儀表板的裝置標籤中，尋找要配置的 Jasper 連接裝置。
 2. 選取裝置，以開啟*裝置往下探查*視圖。
 3. 向下捲動至*延伸配置*。
 4. 使用下列 JSON 格式來輸入延伸配置，然後按一下**確認變更**，以儲存配置。  

```json  
    {
        "jasper": {
            "iccid": "string"
        }
    }

```

順利配置組織之後，*延伸規格*區段會顯示在*裝置往下探查*視圖中的*延伸配置*區段之下。

## AT&T
{: #att}

### 支援的 AT&AMP;T 作業

AT&AMP;T 延伸規格可讓您執行下列 AT&AMP;T 作業：

- 檢視整體 AT&AMP;T 資料
  - 顯示：狀態、費率方案、月初至今的資料使用情形、月初至今的 SMS 使用情形、月初至今的語音使用情形、超額限制、新增日期和修改日期。
- 變更 SIM 啟動狀態。
  - 從下列項目中選取：庫存、啟動就緒、已啟動、已關閉、已淘汰。
- 檢視 SIM 使用情形
  - 顯示：週期開始日期、可入帳資料和資料總計、可入帳 SMS 和 SMS 總計、可入帳語音和語音總計。
  - 週期開始日期可使用 YYYY-MM-DD 格式來設定。
- 傳送 SMS 至 SIM
- 變更費率方案

### AT&AMP;T 的配置

為了能夠將 {{site.data.keyword.iot_short_notm}} 組織連接至 AT&AMP;T，您必須完成組織配置和裝置配置。

若要配置 {{site.data.keyword.iot_short_notm}} 平台，請完成下列步驟。

1. 啟用 AT&AMP;T 延伸規格。若要讓 AT&AMP;T 與 {{site.data.keyword.iot_short_notm}} 組織整合，請完成下列步驟：
  1. 從 {{site.data.keyword.iot_short_notm}} 儀表板選取**延伸規格**。
  2. 在**延伸規格**頁面中，按一下**新增延伸規格**。
  3. 按一下 AT&AMP;T 旁的**新增**。
  4. 輸入 AT&AMP;T 使用者名稱、密碼、存取鍵及網域 ID。
  5. 按一下**完成**。

為了能夠將 {{site.data.keyword.iot_short_notm}} 組織與 AT&T 帳戶連接，必須先完成兩個配置階段。先完成組織配置，然後再配置您的裝置。


2. 配置您的裝置
您可以配置同時連接 {{site.data.keyword.iot_short_notm}} 組織和 AT&T 帳戶的裝置，以在 {{site.data.keyword.iot_short_notm}} 儀表板中顯示 AT&T 的資料。
**重要事項：**在「新增裝置」程序中無法套用 AT&T 配置，只有先前已連接的裝置可以用 AT&T 來配置。若要配置 AT&T 連接的裝置，請完成下列步驟：
 1. 在 {{site.data.keyword.iot_short_notm}} 儀表板的裝置標籤中，尋找要配置的 AT&T 連接裝置。
 2. 選取裝置，以開啟*裝置往下探查*視圖。
 3. 向下捲動至*延伸配置*。
 4. 使用下列 JSON 格式來輸入延伸配置，然後按一下**確認變更**，以儲存配置。  

```json  
    {
        "atnt": {
            "iccid": "string"
        }
    }

```

順利配置組織之後，*延伸規格*區段會顯示在*裝置往下探查*視圖中的*延伸配置*區段之下。

<!--
## ARM mbed connector
{: #arm}

The ARM mbed connector is an extension that allows you to connect your ARM mbed device to your {{site.data.keyword.iot_short_notm}}. The ARM mbed extension is allows the ARM mbed portal and the {{site.data.keyword.iot_short_notm}} to send and receive data from the ARM mbed portal.

### Setup Configuration


1. Enable the ARM mbed connector extension. To enable the ARM mbed connector extension complete the following steps:
  1. From the {{site.data.keyword.iot_short_notm}} dashboard, select **Settings** and navigate to **Extensions**.
  2. In the **Extensions** menu, click **Add Extension**.
  3. Click **Add** next to ARM mbed connector extension.
  4. Enter your ARM mbed access key and domain ID. You can find these by using the ARM mbed portal at https://connector.mbed.com.
  5. Check the credentials are correct by clicking the **Check Connection** button.
  6. Click **Done**.

### Payload Format

There are two types of incoming messages from the ARM mbed platform, notifications and asynchronous responses. The {{site.data.keyword.iot_short_notm}} can send commands to devices that are connected to the ARM mbed platform.

#### Notifications

Notifications are generated by changes in device or sensor data. After the {{site.data.keyword.iot_short_notm}} processes the message, it is to the device event topic in the same way as a device connected directly to the {{site.data.keyword.iot_short_notm}}. The event type used for notifications originating on devices connected to the ARM mbed platform is `notify`.

The following code sample shows the payload format for a notification sent by the ARM mbed platform API:

```
{
  "ep":<endpoint/deviceID>,
  "path":<resource path>,
  "ct":<content type>,
  "payload":<Base64 encoded payload>,
  "max-age":<how long the payload is valid, in seconds>
}
```

#### Asynchronous responses

When the {{site.data.keyword.iot_short_notm}} sends a command to a device connected to the ARM mbed platform, the device sends a confirmation message back to the {{site.data.keyword.iot_short_notm}}. This confirmation message is called an _asynchronous response_ and uses the event type `asyncResponse`.

The following code sample shows the payload format for an asynchronous response sent by the ARM mbed cloud service:

```
{
  "id":<transaction id>,
  "status":<200 is command was sucessfully executed. Check other HTTP response codes>,
  "ct":<content type>,
  "max-age":<how long the payload is valid, in seconds>,
  "payload":<base64 encoded payload>,
  "ep":<endpoint/deviceID affected by the command>,
  "path":<resource path affected by the command>
}
```

#### Sending commands to the ARM mbed platform

The {{site.data.keyword.iot_short_notm}} can send commands to devices connected to the ARM mbed platform. Commands sent to the ARM mbed platform it must use the following JSON format.

```
{
  "method":<PUT or POST>,
  "deviceId":<endpoint/deviceId>,
  "resourceId":<resource path>,
  "payload": <Base64 encoded payload>
}
```

The payload should be published to the following topic:

```
iot-2/type/<device_type>/id/<deviceId>/cmd/<command_type>/fmt/<command_format>
```
-->

## Orange
{: #orange}

Orange 延伸規格可讓您檢視連接至 {{site.data.keyword.iot_short_notm}} 並已安裝 Orange SIM 卡之裝置中的 SIM 卡資料。

https://developer.ibm.com/iotplatform/2016/03/30/watson-iot-platform-integration-with-orange-beta/

### 支援的 Orange 作業

如果您的裝置連接至 {{site.data.keyword.iot_short_notm}} 服務，並有 Orange SIM 卡，則您可以使用 Orange 延伸規格來檢視下列 SIM 卡資料：

- SIM 序號
- 啟動狀態
- 前次狀態變更
- 前次狀態重新整理
- 位置狀態

### Orange 的配置



若要啟用 Orange 延伸規格，請執行下列動作：

1. 從 {{site.data.keyword.iot_short_notm}} 儀表板選取**延伸規格**。
2. 在**延伸規格**頁面中，按一下**新增延伸規格**。
3. 按一下 Orange 延伸規格旁的**新增**。
4. 輸入您的 Orange 使用者名稱和密碼。
6. 按一下**完成**。

啟用 Orange 延伸規格之後，必須配置含有 Orange SIM 卡的每一個裝置，以顯示 Orange SIM 資料。

1. 在 {{site.data.keyword.iot_short_notm}} 儀表板的裝置標籤中，尋找要配置的 Orange SIM 裝置。
2. 選取裝置，然後向下捲動至*延伸配置*。
3. 使用下列 JSON 格式來輸入延伸配置，然後按一下**確認變更**，以儲存配置。

```  
    {
        "orange": {
            "serialnumber": "<serial number of Orange SIM>"
        }
    }

```
順利配置組織之後，*延伸規格*區段會顯示在*裝置往下探查*視圖中的*延伸配置*區段之下。

## 裝置管理延伸規格
{: #device_mgmt}

裝置管理是 {{site.data.keyword.iot_short_notm}} 的核心功能，不過，可加以延伸，以開發其他功能。

裝置管理延伸規格可讓您安裝裝置管理的自訂功能。如需自訂裝置管理功能的相關資訊，請參閱[裝置管理自訂延伸規格](../../devices/device_mgmt/custom_actions.html){: new_window}。

## 區塊鏈
{: #blockchain}

{{site.data.keyword.iot_short_notm}} 與區塊鏈可讓 IoT 裝置提供資料給區塊鏈交易，這樣會將資料儲存在區塊鏈的不可變分類帳中，並將它用在智慧型合約商業規則中。{{site.data.keyword.iot_short_notm}} 會將裝置資料對映至區塊鏈智慧型合約所需的資料格式，並將它傳遞至區塊鏈網狀架構，以儲存在區塊鏈分類帳中。

### 支援的區塊鏈作業
- 以裝置事件來觸發智慧型合約更新項目。
- 執行智慧型合約商業邏輯，使用裝置事件資料來更新分類帳狀態。
- 使用監視使用者介面來監視區塊鏈、交易和分類帳狀態。

### 區塊鏈的配置

{{site.data.keyword.iot_short_notm}} 區塊鏈整合是 {{site.data.keyword.iot_short_notm}} 中預設不啟動的服務供應項目。若要在您的環境中啟動此功能，請完成下列步驟：
 1. 從 {{site.data.keyword.iot_short_notm}} 儀表板選取**設定**，並導覽至**延伸規格**。
 2. 按一下 Blockchain 延伸規格旁的**告訴我更多**鏈結，以移至「IoT 區塊鏈服務供應項目」頁面。
 3. 填寫並提交服務要求表單。核准服務通常需要大約一天。您的要求通過核准之後，您會收到一封電子郵件，指示您如何在 {{site.data.keyword.iot_short_notm}} 組織中啟動區塊鏈整合。
 5. 回到您組織的 {{site.data.keyword.iot_short_notm}} 儀表板，以完成設定。如需相關資訊，請參閱 [{{site.data.keyword.iot_short_notm}} 區塊鏈整合](../../bl_blockchain_integration.html)。
