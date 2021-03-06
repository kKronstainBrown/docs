---

copyright:
  years: 2015, 2016

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# デバイス・モデル
{: #device_model}
最終更新日: 2016 年 9 月 13 日{: .last-updated}

デバイス・モデルとは、デバイスのメタデータと管理特性を記述するものです。{{site.data.keyword.iot_full}} のデバイス・データベースは、
デバイス情報のマスター・ソースです。アプリケーションと管理対象デバイスは、ロケーションの変更やファームウェア更新の進行状況を含む更新情報をデータベースに送信できます。{{site.data.keyword.iot_short_notm}} がそうした更新情報を受信すると、すぐにデバイス・データベースが更新され、アプリケーションでその情報を使用できるようになります。

**注:** 管理拡張を除き、デバイス・モデルはすべて、管理対象デバイスでも非管理対象デバイスでも使用可能です。ただし、非管理対象デバイスからデータベース内のデバイス・モデルを直接更新することはできません。

## デバイスの識別
{: #device_id}

すべてのデバイスには、`typeId` と `deviceId` という属性があります。通常、`typeId` はデバイスのモデルを表し、`deviceId` はそのシリアル番号を表します。{{site.data.keyword.iot_short_notm}} 組織内で、
`typeId` と `deviceId` の組み合わせがデバイスごとに固有でなければなりません。

さらに、{{site.data.keyword.iot_short_notm}} はデバイスごとに、`clientId` という別の ID を構成します。`clientId` は、`organizationId`、またデバイスの `typeId` 値と `deviceId` 値に基づくものであり、個々のデバイスを間違いなく特定する手段となります。

それらの ID で使用できる文字は、各種通信プロトコルや REST API と互換性をもたせるため、制限されています。以下に示すオプションのデバイス ID は、
デバイス管理プロトコルでも使用されます。

- deviceInfo.manufacturer
- deviceInfo.serialNumber
- deviceInfo.model
- deviceInfo.deviceClass
- deviceInfo.description

ID の詳細や他のデバイス管理規格の同等の ID の説明については、[属性](#attributes)を参照してください。


## ID とデバイス・タイプ
{: #id_and_device_types}

{{site.data.keyword.iot_short_notm}} に接続するあらゆるデバイスには、デバイス・タイプが関連付けられています。デバイス・タイプとは、特性や動作を共有するデバイス・グループのことです。

デバイス・タイプには、一連の属性があります。デバイスを {{site.data.keyword.iot_short_notm}} に追加するとき、そのデバイス・タイプの属性がテンプレートとして使用され、テンプレートはデバイス固有の属性によってオーバーライドされます。例えば、デバイス・タイプに、製造時のファームウェア・バージョンを反映する `deviceInfo.fwVersion` 属性の値があり、デバイスの追加時にこの値がデバイス・タイプからデバイスにコピーされるという事例が考えられます。デバイスが追加される際に、`deviceInfo.fwVersion` の値が既に存在する場合、それはデバイス・タイプによってオーバーライドされません。

デバイス・タイプが更新された場合、登録される新しいデバイスのみ、デバイス・タイプ・テンプレートに対する修正内容を継承します。対象デバイス・タイプの既存のデバイスは影響を受けず、変更されることはありません。


## 属性
{: #attributes}

{{site.data.keyword.iot_short_notm}} のデバイスに適用可能な属性のリストを、以下の表に示します。さらに、イタリック体の属性はデバイス・タイプにも適用できます。

- API/DMA: API/デバイス管理エージェントによって更新可能

  - R: 読み取り専用
  - W: 読み取りと書き込み
  - A: 付加

属性                        | タイプ       | 説明                                       | API | DMA   
------------- | ------------- | ------------- | ------------- | -------------  
 clientId                         | ストリング     | MQTT 接続で使用されるクライアント ID          |  R  |     
 *typeId*                         | ストリング     | デバイス・タイプ                                       |  R  |     
 deviceId                         | ストリング     | デバイス ID                                         |  R  |     
 classId                          | ストリング     | デバイスのクラス (「Device」または「Gateway」)           |  R  |     
 gatewayTypeId                    | ストリング     | このデバイスが使用するゲートウェイのタイプ ID       |  R  |     
 gatewayId                        | ストリング     | このデバイスが使用するゲートウェイのデバイス ID     |  R  |     
 status.alert                     | ブール値    | デバイスにアラートが存在するかどうか                   |  W  |     
 *deviceInfo.serialNumber*        | ストリング     | デバイスのシリアル番号                   |  W  |  W    
 *deviceInfo.manufacturer*        | ストリング     | デバイスの製造メーカー                    |  W  |  W   
 *deviceInfo.model*               | ストリング     | デバイスのモデル                           |  W  |  W  
 *deviceInfo.deviceClass*         | ストリング     | デバイスのクラス                           |  W  |  W  
 *deviceInfo.description*         | ストリング     | デバイスの記述名                |  W  |  W  
 *deviceInfo.fwVersion*           | ストリング     | デバイスの現在既知のファームウェア・バージョン    |  W  |  W  
 *deviceInfo.hwVersion*           | ストリング     | デバイスのハードウェア・バージョン                |  W  |  W  
 *deviceInfo.descriptiveLocation* | ストリング     | 部屋番号や建物番号、地理的領域などの場所の説明      |  W  |  W  
 *metadata*                       | 複合    | フリー・フォームのメタデータ                                |  W  |  W  
 added.auth.id                    | ストリング     | デバイスを追加した ID                          |  R  |     
 added.dateTime                   | ストリング     | ISO8601 日時: デバイスが追加された日時 |  R  |     
 refs.diag.errorCodes             | ストリング     | エラー・コードの診断拡張の URI (存在する場合) |  R  |     
 refs.diag.logs                   | ストリング     | ログの診断拡張の URI (存在する場合)        |  R  |     
 refs.location                    | ストリング     | ロケーション拡張の URI (存在する場合)             |  R  |     
 refs.mgmt                        | ストリング     | 管理拡張の URI (存在する場合)                 |  R  |     



## 拡張属性
{: #extended_attributes}

上記の『属性』セクションのリストに示されているコア属性に加えて、コア・デバイス・モデルの拡張として
扱われる追加属性があります。デバイスに関する簡単な照会では、このような拡張ではなくコア・デバイス・モデルの情報が返されます。
拡張の情報は、明示的に指定して要求する必要があります。



拡張名    | 属性の接頭部 | 目的      
------------- | ------------- | -------------                                         
 診断       | diag                 | エラー・ログと診断情報                 
 ロケーション          | location             | デバイスの場所 (定期的に更新される可能性がある)
 デバイス管理 | mgmt                 | デバイス管理アクション (ファームウェア更新など)       


### 診断拡張


診断属性はオプションであり、エラー・ログ情報が存在するデバイスにのみ存在します。これらの属性は、{{site.data.keyword.iot_short_notm}} への接続をトラブルシューティングするためではなく、デバイスの問題を診断するためのものです。これらの属性の情報を取得するには、属性を個別に照会する必要があります。これらの属性に保管されている情報は非常に大きい可能性があるためです。

診断ログ情報は一連の項目を配列にまとめたもので、そこには API を使用して項目を追加することができますが、追加すると、診断ログを管理可能なサイズに維持するために古い項目が失われる可能性があります。各項目には、メッセージ、重大度の標識、タイム・スタンプ、そしてオプションとしてデータのバイト配列が含まれます。


属性            | タイプ       | 説明                                                 | API | DMA
------------- | ------------- | ------------- | ------------- | -------------
 diag.errorCodes[]    | 整数の</br>配列  | エラー・コードの配列                                        |  A  |  A  
 diag.log[]           | 配列      | 診断データの配列                                    |  A  |  A  
 diag.log[].message   | ストリング     | 診断メッセージ                                          |     |     
 diag.log[].timestamp | ストリング     | ISO8601 日時: ログ・エントリーの日時               |     |     
 diag.log[].logData   | ストリング     | バイト: 診断データ、base-64 エンコード                      |     |     
 diag.log[].severity  | 数値     | メッセージの重大度。0: 通知、1: 警告、2: エラー |     |     


### ロケーション拡張

これらの属性はオプションであり、ロケーション情報が存在するデバイスにのみ存在します。ロケーション情報は個別に保管されます。そのため、モバイル・デバイスなどのように情報が頻繁に更新される場合に、動的な情報に適したストレージ・メカニズムを使用できるようになっています。

頻繁にロケーションを更新することがきわめて重要なソリューションでは、ロケーションをデバイスのイベント・ペイロードの一部として扱う必要があります。これにより、更新頻度を上げ、履歴保管と分析を単純化することができます。


属性                 | タイプ   | 説明                                             | API | DMA
------------- | ------------- | ------------- | ------------- | -------------
 location.longitude        | 数値 | WGS84 を使用した経度 (10 進度数)                |  W  |  W  
 location.latitude         | 数値 | WGS84 を使用した緯度 (10 進度数)                 |  W  |  W  
 location.elevation        | 数値 | WGS84 を使用した高度 (メートル)                         |  W  |  W  
 location.measuredDateTime | ストリング |ISO8601 日時: ロケーション測定の日時 |  W  |  W  
 location.updatedDateTime  | ストリング | ISO8601 日時: 日時                        |  R  |     
 location.accuracy         | 数値 | 位置の精度 (メートル)                      |  W  |  W  


### デバイス管理拡張

`mgmt.` 属性は管理対象デバイスにのみ存在します。管理対象デバイスが休止されると、そのデバイスは非管理対象になり、`mgmt.` 属性は削除されます。`mgmt.` 属性は、デバイス管理要求を処理した結果として {{site.data.keyword.iot_short_notm}} により設定されます。API を使用してこの属性を直接書き込むことはできません。

デバイスには、管理対象デバイスとしての状況によって定義される管理ライフサイクルがあります。デバイス管理プロトコルを使用して「デバイスを管理」要求を送信するのは、デバイス上のデバイス管理エージェントです。デバイスが多数存在する環境で機能不良デバイスに対処するために、「デバイスを管理」要求を定期的に送信するように管理対象デバイスを設定することができます。これにより、デバイスが休止したことを {{site.data.keyword.iot_short_notm}} が検知できるようになります。この機能を活用するために、「デバイスを管理」要求にはオプションのライフタイム・パラメーターが用意されています。{{site.data.keyword.iot_short_notm}} は、ライフタイムが指定された「デバイスを管理」要求を受け取ると、次回の「デバイスを管理」要求が必要になるまでの時間を計算し、その時間を `mgmt.dormantDateTime` 属性に保管します。


属性                      | タイプ    | 説明                                            | API | DMA
------------- | ------------- | ------------- | ------------- | -------------
 mgmt.dormant                   | ブール値 | デバイスが休止になったかどうか                  |  R  |     
 mgmt.dormantDateTime           | ストリング  | ISO8601 日時: 管理対象デバイスが休止になる日時   |  R  |     
 mgmt.lastActivityDateTime      | ストリング  | ISO8601 日時: 最後のアクティビティーの日時 (定期的に更新される)    |  R  |     
 mgmt.supports.deviceActions    | ブール値 | デバイスがリブートや工場出荷時設定にリセットのアクションをサポートするかどうか  |  R  |     
 mgmt.supports.firmwareActions  | ブール値 | デバイスがファームウェア・ダウンロードやファームウェア更新のアクションをサポートするかどうか    |  R  |     
 mgmt.firmware.version          | ストリング  | デバイスのファームウェア・バージョン              |  R  |  W  
 mgmt.firmware.name             | ストリング  | デバイスで使用するファームウェアの名前      |  R  |  W  
 mgmt.firmware.uri              | ストリング  | ファームウェア・イメージをダウンロードできる URI |  R  |  W  
 mgmt.firmware.verifier         | ストリング  | ファームウェア・イメージの整合性を検証するためのチェックサムなどのベリファイヤー |  R  |  W  
 mgmt.firmware.state            | 数値  | ファームウェア・ダウンロードの状態を示します               |  R  |  W  
 mgmt.firmware.updateStatus     | 数値  | 更新の状況を示します                     |  R  |  W  
 mgmt.firmware.updatedDateTime  | ストリング  | ISO8601 日時: 最後の更新の日付                 |  R  |     
