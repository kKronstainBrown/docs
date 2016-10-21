---

copyright:
  years: 2015, 2016

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# API REST HTTP per le applicazioni
{: #api}

Ultimo aggiornamento: 07 settembre 2016
{: .last-updated}

Utilizza l'API REST HTTP {{site.data.keyword.iot_full}} per creare e personalizzare le applicazioni che interagiscono con la tua organizzazione su {{site.data.keyword.iot_short_notm}}.
{:shortdesc}

## Funzionalità
{: #capabilities}

L'API REST HTTP {{site.data.keyword.iot_short_notm}} supporta le seguenti funzionalità e funzioni per le applicazioni:

- Richiamo informazioni sull'organizzazione
- Operazioni sul dispositivo in blocco (elenco, aggiunta e rimozione)
- Operazioni di tipo dispositivo (elenco, creazione, eliminazione, visualizzazione dettagli e aggiornamento)
- Operazioni dispositivo (elenco, creazione, eliminazione, visualizzazione dettagli, aggiornamento, visualizzazione ubicazione e visualizzazione delle informazioni di gestione)
- Operazioni di diagnostica del dispositivo (eliminazione log, richiamo log, aggiunta informazioni log, eliminazione log, ottenimento di log specifici, annullamento di codici di errore, ottenimento di codici di errore e aggiunta di codici di errore)
- Determinazione del problema di connessione (elenco degli eventi di log della connessione del dispositivo)
- Ultima cache evento (visualizzazione dell'ultimo evento per un dispositivo specifico)
- Operazioni di richiesta di gestione del dispositivo (elenco delle richieste di gestione del dispositivo, inizializzazione delle richieste, annullamento dello stato della richiesta, ottenimento dei dettagli di una richiesta, elenco di tutti gli stati della richiesta per dispositivo e ottenimento dello stato della richiesta per un dispositivo specifico)
- Operazioni di gestione dell'utilizzo (richiamo della quantità totale di dati utilizzata)
- Pubblicazione di un evento del dispositivo (beta)
- Query dello stato del servizio (richiamo degli stati del servizio per organizzazione)

## Accesso all'API REST HTTP
{: #api_link}

Per accedere all'API REST HTTP {{site.data.keyword.iot_short_notm}} e ottenere ulteriori informazioni su come creare e personalizzare le tue applicazioni, vai all'indirizzo  https://docs.internetofthings.ibmcloud.com/swagger/v0002.html.

L'unica versione dell'API REST HTTP {{site.data.keyword.iot_short_notm}} supportata è la versione 2. Assicurati che le tue soluzioni {{site.data.keyword.iot_short_notm}} utilizzino la versione 2.



# API di messaggistica REST HTTP per le applicazioni 
{: #rest_messaging_api}

## Pubblicazione di eventi e comandi
{: #event_command_publication}

In aggiunta all'utilizzo del protocollo di messaggistica MQTT, puoi anche configurare le tue applicazioni a pubblicare eventi e comandi per {{site.data.keyword.iot_short_notm}} nell'HTTP utilizzando uno dei seguenti comandi API REST HTTP:

### Richiesta POST event non sicura
<pre class="pre">http://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:1883/api/v0002/application/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/events/<var class="keyword varname">eventId</var></pre>
{: codeblock}

### Richiesta POST event sicura 
<pre class="pre">https://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:8883/api/v0002/application/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/events/<var class="keyword varname">eventId</var></pre>
{: codeblock}

### Richiesta POST command non sicura 
<pre class="pre">http://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:1883/api/v0002/application/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/commands/<var class="keyword varname">eventId</var></pre>
{: codeblock}

### Richiesta POST command sicura 
<pre class="pre">https://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:8883/api/v0002/application/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/commands/<var class="keyword varname">eventId</var></pre>
{: codeblock}

Se stai collegando un dispositivo o un'applicazione al servizio Quickstart, sostituisci **orgId** con la stringa 'quickstart'.

NOTA: mentre le applicazioni possono riutilizzare una connessione HTTP per pubblicare eventi o comandi a dispositivi diversi, l'intestazione HTTP dell'autorizzazione non può essere modificata.

### Autenticazione

Tutte le richieste devono includere un'intestazione di autorizzazione. L'autenticazione di base è l'unico metodo supportato. Le applicazioni sono autenticate utilizzando le chiavi API. Quando un'applicazione effettua una richiesta tramite l'API REST HTTP {{site.data.keyword.iot_short_notm}}, sono necessarie le seguenti credenziali:

```
username = API key (for example, a-orgId-a84ps90Ajs)
password = Authentication token
```

### Intestazioni richiesta per tipo di contenuto

Deve essere fornita un'intestazione della richiesta `Content-Type` con la richiesta. La seguente tabella mostra come i tipi supportati sono associati ai formati interni di {{site.data.keyword.iot_short_notm}}.

|Intestazione Content-Type |Formato in {{site.data.keyword.iot_short_notm}}|
|:---|:---|
|testo/semplice|"text"
|applicazione/json| "json"
|applicazione/xml | "xml"
|applicazione/octet-stream|"bin"

### QOS (quality of service)

Simile al livello di sicurezza di distribuzione 0 di QOS (quality of service) MQTT "at most once", la messaggistica REST HTTP fornisce la fornitura del messaggio non persistente ma verifica che la richiesta sia corretta e che possa essere distribuita al server prima che invii la risposta HTTP. Un risposta che contiene un codice di stato HTTP di 200 conferma che il messaggio è stato distribuito al server. Quando utilizzi il livello di QOS (quality of service) MQTT "at most once" o l'equivalente HTTP per distribuire i messaggi dell'evento, il dispositivo o l'applicazione deve implementare la logica del nuovo tentativo per garantire la distribuzione.


Per ulteriori informazioni sul protocollo MQTT e sui livelli di QOS (quality of service) per {{site.data.keyword.iot_short_notm}}, consulta [Messaggistica MQTT](../reference/mqtt/index.html).
