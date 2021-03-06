---

copyright:
  years: 2016

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen:.screen}
{:codeblock:.codeblock}

# Introduzione a {{site.data.keyword.mobileanalytics_short}} (sperimentale)  

{: #gettingstartedtemplate}
*Ultimo aggiornamento: 1 agosto 2016*
{: .last-updated}

Utilizza il servizio {{site.data.keyword.mobileanalytics_full}} per misurare lo stato, la modalità di funzionamento e il contesto delle tue applicazioni mobili, degli utenti delle applicazioni mobili e dei dispositivi mobili.
{: shortdesc}

Per iniziare a lavorare rapidamente con il servizio {{site.data.keyword.mobileanalytics_short}}, attieniti alla seguente procedura:

1. Dopo che hai [creato un'istanza](https://console.{DomainName}/docs/services/reqnsi.html#req_instance) del servizio {{site.data.keyword.mobileanalytics_short}},
puoi accedere alla console {{site.data.keyword.mobileanalytics_short}} facendo clic sul tuo tile nella sezione Servizi del dashboard {{site.data.keyword.Bluemix}}.

  **Importante:** quando apri inizialmente il tuo servizio Mobile Analytics appena creato, viene visualizzata una finestra per confermare che tu consenti a {{site.data.keyword.Bluemix_notm}} di fornire le informazioni su di te necessarie al servizio per poter convalidare la tua identità. Fai clic su **Conferma** per continuare alla console {{site.data.keyword.mobileanalytics_short}}. Se annulli, la console {{site.data.keyword.mobileanalytics_short}} non verrà aperta.
2. Installa gli [SDK client](install-client-sdk.html) {{site.data.keyword.mobileanalytics_short}}.
3. Importa gli SDK client e inizializzali con il seguente frammento di codice per registrare l'analisi dell'utilizzo.
  #### Android
  {: #android-initialize}
  1. Importa l'SDK client:
		
		```
		import com.ibm.mobilefirstplatform.clientsdk.android.core.api.*;
		import com.ibm.mobilefirstplatform.clientsdk.android.analytics.api.*;
		```

  2. Ottieni il tuo valore [Chiave di accesso](sdk.html#analytics-clientkey).
  3. Inizializza l'SDK client all'interno del tuo codice dell'applicazione per registrare l'analisi dell'utilizzo e le sessioni dell'applicazione. 
		
		```Java
		try {
		        BMSClient.getInstance().initialize(this.getApplicationContext(), "", "", BMSClient.REGION_US_SOUTH);
			}
			catch (MalformedURLException e) {
		        Log.e("your_app_name","URL should not be malformed:  " + e.getLocalizedMessage());
		    }
		   Analytics.init(getApplication(), "your_app_name", "your_access_key", Analytics.DeviceEvent.LIFECYCLE);
		```
    Il parametro **bluemixRegion** specifica quale distribuzione Bluemix stai utilizzando, ad esempio `BMSClient.REGION_US_SOUTH`, `BMSClient.REGION_UK` o `BMSClient.REGION_SYDNEY`.

  #### iOS
  {: #ios-initialize}
  1. Importa i framework `BMSCore` e `BMSAnalytics`:

    ```
    import BMSCore
    import BMSAnalytics
    ```

  2. Ottieni il tuo valore [Chiave di accesso](sdk.html#analytics-clientkey).

  3. Inizializza l'SDK client all'interno del tuo codice dell'applicazione per registrare l'analisi dell'utilizzo e le sessioni dell'applicazione. 
	
	```Swift
	BMSClient.sharedInstance.initializeWithBluemixAppRoute("nil",bluemixAppGUID: "nil", bluemixRegion: BMSClient.REGION_US_SOUTH) //Puoi modificare la regione
	Analytics.initializeWithAppName("your_app_name", accessKey: "your_access_key", deviceEvents: DeviceEvent.LIFECYCLE)
	```

    Il parametro **bluemixRegion** specifica quale distribuzione Bluemix stai utilizzando, ad esempio `BMSClient.REGION_US_SOUTH`, `BMSClient.REGION_UK` o `BMSClient.REGION_SYDNEY`.

4. Invia l'analisi di utilizzo registrata al servizio Mobile Analytics. Un modo semplice per verificare la tua analisi consiste nell'eseguire il seguente codice quando viene avviata la tua applicazione:

	#### Android
	{: #android-send}
	
	Puoi aggiungere il metodo `Analytics.send()` nel metodo `onCreate` dell'attività principale nella tua applicazione Android o nell'ubicazione che ritieni più indicata per il tuo progetto.
	
	```
	Analytics.send(new ResponseListener() {
	    @Override
	    public void onSuccess(Response response) {
	        Log.d("your_app_name", "Successfully sent analytics: " + response.toString());
	    }
		
	    @Override
	    public void onFailure(Response response, Throwable throwable, JSONObject jsonObject) {
	        Log.e("your_app_name", "Failed to send analytics: ");
	        if (response != null) {
	            Log.e("your_app_name", response.toString());
	        }
	        if (throwable != null) {
	            Log.e("your_app_name","Stack trace: ", throwable);
	        }
	    }
	});
	```
	
	#### iOS
	{: #ios-send}
	
	
	Utilizza il metodo `Analytics.send` per inviare i dati di analisi al server. Inserisci il metodo `Analytics.send` nel
metodo `application(_:didFinishLaunchingWithOptions:)` del delegato della tua applicazione oppure in un'ubicazione che ritieni più adatta per il tuo progetto. 
		
	```
	Analytics.send { (response: Response?, error: NSError?) in
	  if response?.statusCode == 201 {
	      print("Successfully sent analytics: \(response?.responseText)")
	  }
	  else {
	      print("Failed to send analytics: \(response?.responseText). Error: \(error?.localizedDescription)")
	  }
	}
	```
Consulta l'argomento [Strumentazione della tua applicazione](sdk.html). 5. Compila ed esegui l'applicazione sul tuo emulatore o sul tuo dispositivo.

6. Vai al **Dashboard** {{site.data.keyword.mobileanalytics_short}} per visualizzare l'analisi di utilizzo, come ad esempio i nuovi dispositivi e il numero totale di dispositivi che utilizzano l'applicazione. Puoi inoltre monitorare la tua applicazione con <!-- [creating custom charts](app-monitoring.html#custom-charts), --> [impostando degli avvisi](app-monitoring.html#alerts) e [monitorando gli arresti anomali delle applicazioni](app-monitoring.html#monitor-app-crash). 


# rellinks

## SDK
* [SDK Android](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-android-analytics){: new_window}  
* [SDK iOS](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-analytics){: new_window}
