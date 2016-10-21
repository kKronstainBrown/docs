---

copyright:
  years: 2016

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 使用 {{site.data.keyword.cloudant}} 連接及配置歷程服務  
{: #cloudant_main}
前次更新：2016 年 9 月 16 日
{: .last-updated}

將 {{site.data.keyword.cloudantfull}} 服務連接至 {{site.data.keyword.iot_full}}，可讓您儲存及存取裝置資料。根據選取的儲存區間隔，裝置資料會儲存在每日、每週或每月資料庫中。

開始使用 {{site.data.keyword.cloudant}} 來儲存裝置資料時，會自動建立三個資料庫：一個資料庫是針對現行儲存區間隔建立、一個是針對未來間隔建立，以及一個配置資料庫。設計文件可以新增至配置資料庫，而且會在建立時複製到新的資料庫。到達間隔尾端時，新間隔的裝置資料會儲存在儲存區資料庫中，並為下列間隔建立新的資料庫。

裝置資料在傳送至資料庫時，可以透過其中一種方式（共兩種）儲存。如果資料是有效的 JSON，而且裝置事件的格式設為 `JSON`，則會以下列格式儲存裝置資料：

```json

{
  "_id": "78bf4380-3311-11e6-a747-d7b140d1a70a",
  "_rev": "2-d13912b7c089f060a4ba7369fa86e46f",
  "typeId": "t",
  "deviceType": "0",
  "eventType": "json_payload",
  "format": "json",
  "timestamp": "2016-06-15T16:54:41.464+01",
  "data": {
    "a": 22
  }
}

```

如果裝置資料不是有效的 JSON，或格式未設為 `JSON`，則會以下列格式將裝置資料儲存為 base64 編碼字串，並儲存在 `payload` 欄位下：

```json

{
  "_id": "80f1ce10-3311-11e6-a747-d7b140d1a70a",
  "_rev": "1-bfcbf1e74389fe4188a9425c0cd2575a",
  "payload": "eHh4eHg=",
  "typeId": "t",
  "deviceType": "0",
  "eventType": "non_json_payload",
  "format": "notjson",
  "timestamp": "2016-06-15T16:54:55.217+01"
}

```

## 開始之前  
{: #byb}

將 {{site.data.keyword.cloudant}} 連接至 {{site.data.keyword.iot_short}} 服務之前，請完成下列作業：

- 使用「Bluemix 型錄」，在與 {{site.data.keyword.iot_short_notm}} 相同的 Bluemix 空間中設定 {{site.data.keyword.cloudant}}。

請確定您具有 Bluemix 組織中的開發人員專用權，並且透過 Bluemix 登入。如果您未透過 Bluemix 登入，或沒有此 Bluemix 組織中的開發人員專用權，則無法授權 {{site.data.keyword.cloudant}} 及 {{site.data.keyword.iot_short_notm}} 的連結。

請完成下列步驟，以連接 {{site.data.keyword.cloudant}}：

1. 在 {{site.data.keyword.iot_short}} 儀表板上，按一下導覽列中的**延伸規格**。
2. 在「歷程資料儲存空間」磚中，按一下**設定**。
2. 與 {{site.data.keyword.iot_short}} 服務相同之 Bluemix 空間內的所有可用 {{site.data.keyword.cloudant}} 服務，都會列在「配置歷程資料儲存空間」區段中。
3. 選取您想要連接的 {{site.data.keyword.cloudant}} 服務。
4. 選取 {{site.data.keyword.cloudant}} 配置選項：

  a. 選取儲存區間隔。儲存區間隔可控制建立新資料庫來儲存裝置資料的頻率。使用選取的儲存區間隔，在所選取時區的午夜建立新的儲存區。

  b. 選取時區。所選取時區的時間將會用來決定應該放入裝置資料的儲存區，而不是裝置的本機時間。決定在其中輸入資料的資料庫時，會將正在傳送至 {{site.data.keyword.cloudant}} 之裝置資料的時間戳記轉換成選取的時區。

  c. 選擇資料庫名稱。資料庫名稱將是 `Iotp_<orgID>_<choice>_<bucket_name>`，其中 `<orgID>` 是您的組織 ID、`<choice>` 是您選擇的資料庫名稱，而 `<bucket_name>` 是定義資料庫使用每日、每週還是每月儲存區間隔的字串。`<bucket_name>` 使用下列格式。

  對於每日儲存區間隔，`<bucket_name>` 將是 `yyyy-mm-dd`。
  對於每週儲存區間隔，`<bucket_name>` 將是 `yyyy-www`，其中 `www` 指出週數（例如 `2016-w03`）
  對於每月儲存區間隔，`<bucket_name>` 將是 `yyyy-mm`。

5. 按一下**授權**。
6. 按一下授權對話框中的**確認**。

您的裝置資料現在儲存在 {{site.data.keyword.cloudant}} 中。

## 建立新的設計文件  
{: #design_docs}

新的設計文件包含在配置資料庫中，並且會複製到每個建立的資料庫。配置資料庫名稱是 'Iotp_<orgid>_<choice>_configuration'，其使用與「開始之前」小節之步驟 3b 中所述的資料庫名稱相同的參數。

{{site.data.keyword.iot_short_notm}} 內所含的預設設計文件會實作現行歷程中可用的查詢（不含彙總函數）。

其他設計文件可以新增至配置資料庫，而且會在建立時複製到新的儲存區間隔資料庫。若要將設計文件新增至配置資料庫，請參閱 [Cloudant API 文件](https://docs.cloudant.com/document.html)。

<!--  # Related links
{: #rellinks}
* [Querying your {{site.data.keyword.cloudant}}](link) -->
