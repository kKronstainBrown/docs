---

copyright:
  years: 2016

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# {{site.data.keyword.iot_short_notm}} のトラブルシューティング
{: #ts}
最終更新日: 2016 年 8 月 1 日
{: .last-updated}

{{site.data.keyword.Bluemix_notm}} 上での {{site.data.keyword.iot_full}} の使用についての一般的なトラブルシューティングの質問に対する答えを提供します。
{:shortdesc}

## {{site.data.keyword.iot_short_notm}} への接続の問題
{: #connection_problem}

{{site.data.keyword.iot_short_notm}} への接続が予期せずドロップまたは切断されます。
{:shortdesc}

{{site.data.keyword.iot_short_notm}} に接続しようとすると、デバイスまたはアプリケーションがエラーを受け取ります。
{: tsSymptoms}

同じ clientID と資格情報を使って 2 つのデバイスが接続を試行している可能性があります。clientID ごとに 1 つの固有の接続のみが許可されています。同じ ID を使用して 2 つの同時接続を行うことはできません。アプリケーションは同じ API キーを共有できますが、MQTT ではクライアント ID が常に固有であることが必要です。
{: tsCauses}

この問題は、同じ資格情報を使用して接続を試行する 2 つのデバイスをなくすことで解決できます。
{: tsResolve}

## デバイスが {{site.data.keyword.iot_short_notm}} から断続的に切断されます。
{: #disconnection_problem}

{{site.data.keyword.iot_short_notm}} へのデバイスの接続が予期せず断続的にドロップします。
{:shortdesc}

{{site.data.keyword.iot_short_notm}} サービスに接続しているデバイスが断続的に切断されます。デバイスは再接続しますが、しばらくすると再び予期せずに切断されます。
{: tsSymptoms}

接続時に使用する MQTT の ping オプションの値が小さすぎるため、接続タイムアウトになったと認識されている可能性があります。例えば、クライアント MQTT が正しく設定されていない場合、ping はタイミング良く受信されず、接続が閉じてしまいます。
{: tsCauses}

この問題は、接続の ping パラメーターと KeepAlive パラメーターを正しく設定することで修正できます。   
{: tsResolve}


## {{site.data.keyword.iot_short_notm}} のヘルプとサポートの入手
{: #gettinghelp}

{{site.data.keyword.iot_short_notm}} を使用していて問題や質問がある場合は、情報を検索するか、またはフォーラムで質問を投稿することで役立つ情報を得ることができます。また、サポート・チケットを開くこともできます。

フォーラムを使用して質問するときは、{{site.data.keyword.Bluemix_notm}} 開発チームの目に止まるように、質問にタグを付けてください。

* {{site.data.keyword.iot_short_notm}} を使用したアプリの開発やデプロイに関して技術上の質問がある場合は、質問を[スタック・オーバーフロー](http://stackoverflow.com/search?q=watson-iot+ibm-bluemix){:new_window}に投稿し、質問には「ibm-bluemix」と「watson-iot」のタグを付けてください。
<!--Insert the appropriate dW Answers tag for your service for <service_keyword> in URL below:  -->
* サービスや概説に関する質問は、[IBM developerWorks dW Answers](https://developer.ibm.com/answers/topics/watson-iot/?smartspace=bluemix){:new_window} フォーラムを使用してください。「watson-iot」と「bluemix」のタグを含めてください。

フォーラムの使用について詳しくは、[ヘルプの取得](https://www.{DomainName}/docs/support/index.html#getting-help)を参照してください。

IBM サポート・チケットを開く方法や、サポート・レベルとチケットの重大度については、[サポートへのお問い合わせ](https://www.{DomainName}/docs/support/index.html#contacting-support)を参照してください。
