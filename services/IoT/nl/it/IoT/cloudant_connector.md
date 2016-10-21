---

copyright:
  years: 2016

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Connessione e configurazione di un servizio storico utilizzando un {{site.data.keyword.cloudant}}  
{: #cloudant_main}
Ultimo aggiornamento: 16 settembre 2016
{: .last-updated}

La connessione di un servizio {{site.data.keyword.cloudantfull}} al tuo {{site.data.keyword.iot_full}} ti consente di archiviare e accedere ai tuoi dati del dispositivo. I dati del dispositivo sono archiviati nei database giornalmente, settimanalmente o mensilmente a seconda dell'intervallo bucket selezionato.

Quando inizi ad utilizzare un {{site.data.keyword.cloudant}} per archiviare i dati del dispositivo vengono creati automaticamente tre database, un database viene creato per l'intervallo bucket corrente, uno per l'intervallo in entrata e un database per la configurazione. I documenti di progettazione possono essere aggiunti al database di configurazione e saranno copiati nei nuovi database come vengono creati. Quando viene raggiunto il termine di un intervallo, i dati del dispositivo vengono archiviati nel database bucket per il nuovo intervallo e viene creato un nuovo database per l'intervallo successivo.

Quando i dati del dispositivo sono inviati a un database, possono essere archiviati in due modi. Se i dati sono JSON validi e il formato dell'evento del dispositivo è impostato su `JSON`, i dati del dispositivo saranno archiviati nel seguente formato:

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

Se i dati del dispositivo non sono JSON validi o se il formato non è impostato su `JSON` i dati del dispositivo saranno archiviati come una stringa codificata base64 nel campo `payload` nel seguente formato:

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

## Prima di cominciare  
{: #byb}

Prima di collegare un {{site.data.keyword.cloudant}} al tuo servizio {{site.data.keyword.iot_short}}, completa la seguente procedura:

- Configura {{site.data.keyword.cloudant}} nello stesso spazio Bluemix del tuo {{site.data.keyword.iot_short_notm}} utilizzando il catalogo Bluemix.

Assicurati di disporre dei privilegi da sviluppatore nell'organizzazione Bluemix e di essere registrato tramite Bluemix. Se non sei registrato con Bluemix o non disponi dei privilegi da sviluppatore in questa organizzazione Bluemix, non sarai in grado di autorizzare il bind di {{site.data.keyword.cloudant}} e di {{site.data.keyword.iot_short_notm}}.

Completa la seguente procedura per collegarti a {{site.data.keyword.cloudant}}:

1. Nel tuo dashboard {{site.data.keyword.iot_short}} fai clic su **Extensions** nella barra di navigazione.
2. Nel tile Historical Data Storage, fai clic su **Setup**.
2. Vengono elencati tutti i servizi {{site.data.keyword.cloudant}} disponibili nello stesso spazio Bluemix del tuo servizio {{site.data.keyword.iot_short}} nella sezione Configure historical data storage.
3. Seleziona il servizio {{site.data.keyword.cloudant}} che desideri collegare.
4. Seleziona le tue opzioni di configurazione {{site.data.keyword.cloudant}}:

  a. Seleziona un intervallo bucket. L'intervallo bucket controlla quanto frequentemente vengono creati nuovi database per memorizzare i dati del dispositivo. I nuovi bucket sono creati a mezzanotte nel fuso orario selezionato utilizzando l'intervallo bucket selezionato.

  b. Seleziona un fuso orario. L'ora nel fuso orario selezionato che sarà utilizzata per determinare in quale bucket devono essere posizionati i dati del dispositivo, non l'ora locale del dispositivo. Le date/ore nei dati del dispositivo che stanno venendo configurate da {{site.data.keyword.cloudant}} saranno convertite nel fuso orario selezionato una volta deciso in quale quale database saranno inseriti i dati.

  c. Scegli un nome per il database. Il nome del database sarà `Iotp_<orgID>_<choice>_<bucket_name>` dove `<orgID>` è il tuo ID dell'organizzazione, `<choice>` è il nome del database di tua scelta e `<bucket_name>` è una stringa che definisce se il database utilizza un intervallo bucket giornaliero, settimanale o mensile. Il `<bucket_name>` utilizza il seguente formato.

  Per gli intervalli bucket giornalieri, `<bucket_name>` sarà `yyyy-mm-dd`.
  Per gli intervalli bucket settimanali `<bucket_name>` sarà `yyyy-www` dove `www` indica un numero della settimana, ad esempio `2016-w03`
  Per gli intervalli bucket mensili `<bucket_name>` sarà `yyyy-mm`.

5. Fare clic su **Authorize**.
6. Fai clic su **Confirm** nella casella di dialogo dell'autorizzazione.

I tuoi dati del dispositivo sono ora archiviati nel tuo {{site.data.keyword.cloudant}}.

## Creazione di nuovi documenti di progettazione  
{: #design_docs}

I nuovi documenti di progettazione sono contenuti nel database di configurazione e sono copiati in ogni database creato. Il nome del database di configurazione è `Iotp_<orgid>_<choice>_configuration
` utilizzando gli stessi parametri dei nomi database descritti nel passo 3b nella sezione Prima di iniziare.

I documenti di progettazione predefiniti contenuti nelle query di implementazione {{site.data.keyword.iot_short_notm}} disponibili nello storico corrente, a prescindere dalla funzione di riepilogo.

Possono essere aggiunti ulteriori documenti di progettazione nel database di configurazione e saranno copiati nei nuovi database dell'intervallo bucket come vengono creati. Per aggiungere i documenti di progettazione al database di configurazione, consulta la [Cloudant API documentation](https://docs.cloudant.com/document.html).

<!--  # Related links
{: #rellinks}
* [Querying your {{site.data.keyword.cloudant}}](link) -->
