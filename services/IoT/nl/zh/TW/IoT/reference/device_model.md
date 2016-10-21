---

copyright:
  years: 2015, 2016

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# 裝置模型
{: #device_model}
前次更新：2016 年 9 月 13 日 {: .last-updated}

裝置模型說明裝置的 meta 資料及管理性質。{{site.data.keyword.iot_full}} 中的裝置資料庫是裝置資訊的主要來源。應用程式和受管理裝置可以將更新項目（包括位置變更）或韌體更新的進度傳送至資料庫。{{site.data.keyword.iot_short_notm}} 收到這些更新項目之後，就會更新裝置資料庫，而應用程式就能使用該資訊。

**附註：**除了管理延伸規格之外，整個裝置模型都可用於受管理和未受管理的裝置。不過，未受管理裝置無法直接更新其在資料庫中的裝置模型。

## 裝置識別
{: #device_id}

每個裝置都有 `typeId` 和 `deviceId` 屬性。一般而言，`typeId` 代表裝置的模型，而 `deviceId` 可以代表其序號。在您的 {{site.data.keyword.iot_short_notm}} 組織內，每一個裝置的 `typeId` 與 `deviceId` 組合都必須是唯一的。

此外，{{site.data.keyword.iot_short_notm}} 會為每一個裝置建構另一個 ID，稱為 `clientId`。`clientId` 是以 `organizationId` 以及裝置的 `typeId` 和 `deviceId` 值為基礎，提供可明確識別個別裝置的方式。

這些 ID 中可使用的字元受限制，使它們能夠與通訊協定和 REST API 相容。下列選用性裝置 ID 也用於裝置管理通訊協定：

- deviceInfo.manufacturer
- deviceInfo.serialNumber
- deviceInfo.model
- deviceInfo.deviceClass
- deviceInfo.description

如需其相對 ID 於其他裝置管理標準中之 ID 和說明的相關資訊，請參閱[屬性](#attributes)。


## ID 和裝置類型
{: #id_and_device_types}

連接至 {{site.data.keyword.iot_short_notm}} 的每個裝置都與裝置類型相關聯。裝置類型是共用性質或行為的裝置群組。

裝置類型有一組屬性。當裝置新增至 {{site.data.keyword.iot_short_notm}} 時，其裝置類型中的屬性會用來作為裝置特定屬性所置換的範本。例如，裝置類型可能有 `deviceInfo.fwVersion` 屬性的值，它反映製造時的韌體版本，而在新增裝置時，會將此值從裝置類型複製到裝置中。新增裝置時，如果 `deviceInfo.fwVersion` 值已存在，裝置類型不會將其置換。

裝置類型更新之後，只有已登錄的新裝置會繼承裝置類型範本的修改內容。裝置類型的現有裝置不受影響，且保持不變。

## 屬性
{: #attributes}

下表顯示可在 {{site.data.keyword.iot_short_notm}} 中套用至裝置的屬性清單。此外，以斜體表示的屬性也可套用至裝置類型。

- API/DMA：可透過 API/裝置管理代理程式更新

  - R：唯讀
  - W：讀寫
  - A：附加

屬性                        | 類型       | 說明                                       | API | DMA   
------------- | ------------- | ------------- | ------------- | -------------  
 clientId                         | 字串     | 與 MQTT 連線搭配使用的用戶端 ID          |  R  |     
 *typeId*                         | 字串     | 裝置類型                                       |  R  |     
deviceId                         | 字串     | 裝置 ID                                         |  R  |     
classId                          | 字串     | 裝置類別（「裝置」或「閘道」）           |  R  |     
 gatewayTypeId                    | 字串     | 此裝置所使用之閘道的類型 ID       |  R  |     
 gatewayId                        | 字串     | 此裝置所使用之閘道的裝置 ID     |  R  |     
 status.alert                     | 布林    | 裝置是否有警示                   |  W  |     
 *deviceInfo.serialNumber*        | 字串     | 裝置的序號                   |  W  |  W    
 *deviceInfo.manufacturer*        | 字串     | 裝置的製造商                    |  W  |  W   
 *deviceInfo.model*               | 字串     | 裝置的模型                           |  W  |  W  
 *deviceInfo.deviceClass*         | 字串     | 裝置的類別                           |  W  |  W  
 *deviceInfo.description*         | 字串     | 裝置的敘述性名稱                |  W  |  W  
 *deviceInfo.fwVersion*           | 字串     | 裝置上目前已知的韌體版本    |  W  |  W  
 *deviceInfo.hwVersion*           | 字串     | 裝置的硬體版本                |  W  |  W  
 *deviceInfo.descriptiveLocation* | 字串     | 敘述性位置，例如廳室、大廈號碼或地理區域      |  W  |  W  
 *metadata*                       | 複式    | 開放式 meta 資料                                |  W  |  W  
 added.auth.id                    | 字串     | 新增裝置的 ID                          |  R  |     
 added.dateTime                   | 字串     | ISO8601 日期時間：新增裝置的日期和時間 |  R  |     
 refs.diag.errorCodes             | 字串     | 錯誤碼的診斷延伸規格 URI（如果存在的話） |  R  |     
 refs.diag.logs                   | 字串     | 日誌的診斷延伸規格 URI（如果存在的話）        |  R  |     
 refs.location                    | 字串     | 位置延伸規格的 URI（如果存在的話）             |  R  |     
 refs.mgmt                        | 字串     | 管理延伸規格的 URI（如果存在的話）                 |  R  |     



## 延伸屬性
{: #extended_attributes}

除了「屬性」區段上方所列的核心屬性之外，還有其他屬性被視為核心裝置模型的延伸規格。與裝置相關的簡單查詢會從核心裝置模型傳回資訊，但不會傳回延伸規格。來自延伸規格的資訊必須明確要求。


延伸規格名稱    | 屬性的字首 | 目的      
------------- | ------------- | -------------                                         
 診斷       | diag                 | 錯誤日誌與診斷資訊                 
 位置          | location             | 裝置的位置，可能會定期更新
 裝置管理 | mgmt                 | 裝置管理動作，例如：韌體更新       


### 診斷延伸規格


診斷屬性是選用的，只有含錯誤日誌資訊的裝置有這些屬性。這些屬性是要用來診斷裝置問題，而不是要對 {{site.data.keyword.iot_short_notm}} 的連線功能進行疑難排解。為了擷取這些屬性中的資訊，其必須分開查詢，因為這些屬性中儲存的資訊可能非常大。

診斷日誌資訊是個項目的陣列，其可利用 API 來附加項目；不過，這可能會導致之前的項目遺失，使得診斷日誌維持在可管理的大小。每一個項目都包含一則訊息、嚴重性指示、時間戳記，以及選用的資料位元組陣列。


屬性            | 類型       | 說明                                                 | API | DMA
------------- | ------------- | ------------- | ------------- | -------------
 diag.errorCodes[]    | 整數</br> 陣列  | 錯誤碼的陣列                                        |  A  |  A  
 diag.log[]           | 陣列      | 診斷資料的陣列                                    |  A  |  A  
 diag.log[].message   | 字串     | 診斷訊息                                          |     |     
 diag.log[].timestamp | 字串     | ISO8601 日期時間：日誌項目的日期和時間               |     |     
 diag.log[].logData   | 字串     | 位元組：診斷資料，Base-64 編碼                      |     |     
 diag.log[].severity  | 數值     | 訊息嚴重性，0：參考資訊，1：警告，2：錯誤 |     |     


### 位置延伸規格

這些屬性是選用的，只有含位置資訊的裝置有這些屬性。位置資訊會分開儲存，以讓使用儲存體機制時更適合經常更新的動態資訊，例如：行動式裝置的資訊。

若為極度重視頻繁更新位置的解決方案，應該會將位置視為裝置事件有效負載的一部分，以提高更新率、簡化歷程儲存及進行分析。


屬性                 | 類型   | 說明                                             | API | DMA
------------- | ------------- | ------------- | ------------- | -------------
 location.longitude        | 數值 | 經度，使用 WGS84 的十進位制                |  W  |  W  
 location.latitude         | 數值 | 緯度，使用 WGS84 的十進位制                 |  W  |  W  
 location.elevation        | 數值 | 海拔高度，使用 WGS84 的公尺數                         |  W  |  W  
 location.measuredDateTime | 字串 |ISO8601 日期時間：測量位置的日期和時間 |  W  |  W  
 location.updatedDateTime  | 字串 | ISO8601 日期時間：日期和時間                        |  R  |     
 location.accuracy         | 數值 | 位置的正確性（公尺）                      |  W  |  W  


### 裝置管理延伸規格

只有受管理裝置會有 `mgmt.` 屬性。當受管理裝置變成休眠時，就會變成未受管理，而 `mgmt.` 屬性會被刪除。{{site.data.keyword.iot_short_notm}} 會在處理裝置管理要求之後，設定 `mgmt.` 屬性。無法使用 API 直接撰寫這些屬性。

裝置具有管理生命週期，依其狀態定義為受管理裝置。裝置上的裝置管理代理程式會負責使用裝置管理通訊協定來傳送「管理裝置」要求。若要處理大型裝置個體群中的已廢止裝置，可以設定受管理裝置來定期傳送「管理裝置」要求，讓 {{site.data.keyword.iot_short_notm}} 能夠在裝置變成休眠時發出通知。為協助進行這項功能，「管理裝置」要求有一個選用性的生命期限參數。當 {{site.data.keyword.iot_short_notm}} 收到具有生命期限的「管理裝置」要求時，會計算需要另一個「管理裝置」要求之前的時間，並將其儲存在 `mgmt.dormantDateTime` 屬性中。


屬性                      | 類型    | 說明                                            | API | DMA
------------- | ------------- | ------------- | ------------- | -------------
 mgmt.dormant                   | 布林 | 裝置是否已變成休眠                  |  R  |     
 mgmt.dormantDateTime           | 字串  | ISO8601 日期時間：受管理裝置變成休眠的日期和時間   |  R  |     
 mgmt.lastActivityDateTime      | 字串  | ISO8601 日期時間：前次活動的日期和時間，定期更新    |  R  |     
 mgmt.supports.deviceActions    | 布林 | 裝置是否支援「重新開機」及「重設為原廠設定」動作  |  R  |     
 mgmt.supports.firmwareActions  | 布林 | 裝置是否支援「韌體下載」及「韌體更新」動作    |  R  |     
 mgmt.firmware.version          | 字串  | 裝置上的韌體版本              |  R  |  W  
 mgmt.firmware.name             | 字串  | 裝置上使用的韌體名稱      |  R  |  W  
 mgmt.firmware.uri              | 字串  | 可下載韌體映像檔的 URI |  R  |  W  
 mgmt.firmware.verifier         | 字串  | 韌體映像檔用來驗證其完整性的驗證器（如：總和檢查） |  R  |  W  
 mgmt.firmware.state            | 數值  | 指出韌體下載的狀態               |  R  |  W  
 mgmt.firmware.updateStatus     | 數值  | 指出更新的狀態                     |  R  |  W  
 mgmt.firmware.updatedDateTime  | 字串  | ISO8601 日期時間：前次更新的日期                 |  R  |     
