---

copyright:
  years: 2015, 2016

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Connessione ai gateway
{: #IoT_connectGateway}
Ultimo aggiornamento: 28 luglio 2016

Prima di poter iniziare a ricevere i dati dai dispositivi collegati ai tuoi gateway, devi collegare il gateway a {{site.data.keyword.iot_full}}. Il collegamento di un gateway a {{site.data.keyword.iot_short_notm}} implica la creazione di un tipo di dispositivo gateway e la registrazione del gateway con {{site.data.keyword.iot_short_notm}}. Puoi quindi utilizzare le informazioni registrate per collegare il gateway a {{site.data.keyword.iot_short_notm}}.
{:shortdesc}

I gateway sono una classe specializzata di dispositivi in {{site.data.keyword.iot_short_notm}}. I gateway sono utilizzati come punti di accesso a {{site.data.keyword.iot_short_notm}} per altri dispositivi.


## Prima di cominciare
{: #Prerequisites}

I dispositivi gateway dispongono di ulteriori autorizzazioni quando confrontati ai dispositivi regolari e possono eseguire le seguenti funzioni:
- Registrare nuovi dispositivi in Watson IoT Platform
- Inviare e ricevere i propri dati del sensore come un dispositivo collegato direttamente
- Inviare e ricevere i dati al posto di dispositivi collegati ad esso
- Eseguire un agent di gestione del dispositivi, in modo che possa essere gestito e inoltre gestire i dispositivi collegati ad esso.
Per le informazioni per lo sviluppatore del gateway, consulta [Connettività MQTT per i gateway](mqtt.html).

Puoi anche utilizzare i gateway per eseguire analisi edge sui dati che i dispositivi gateway stanno inviando. Per ulteriori informazioni, consulta [Analisi Edge](../edge_analytics.html) e [Installazione dell'agent di analisi edge](#edge).

## Passo 1: registrazione del tuo gateway con {{site.data.keyword.iot_short_notm}}  
{: #register_gateway}

La registrazione di un gateway implica la classificazione del dispositivo come un tipo di gateway, dando al gateway un nome e fornendo le informazioni sul gateway. Quindi fornisci un token di connessione o accetta un token generato da {{site.data.keyword.iot_short_notm}}.

**Suggerimento:** puoi aggiungere un gateway alla volta dal dashboard {{site.data.keyword.iot_short_notm}}, o puoi utilizzare l'API [{{site.data.keyword.iot_short_notm}}](https://docs.internetofthings.ibmcloud.com/swagger/v0002.html#!/Bulk_Operations/post_bulk_devices_add) per aggiungere uno o più gateway contemporaneamente.

Per aggiungere un gateway dal dashboard {{site.data.keyword.iot_short_notm}}:

1. Nel dashboard {{site.data.keyword.iot_short_notm}}, seleziona **Devices**.
2. Fai clic su **Add Device**.
3. Seleziona o crea un tipo di dispositivo per il dispositivo che stai aggiungendo.
Ogni dispositivo collegato a {{site.data.keyword.iot_short_notm}} deve essere associato a un tipo di dispositivo. I tipi di dispositivo sono gruppi di dispositivi che condividono caratteristiche comuni.  
 1. Fai clic su **Create device type** quindi su **Create gateway type**.
 2. Immetti un nome come ad esempio `my_gateway_type` e una descrizione per il tipo di gateway.
 3. Facoltativo: immetti i metadati e gli attributi per il tipo di gateway.
 **Suggerimento:** puoi aggiungere e modificare gli attributi e i metadati successivamente.
 4. Fai clic su **Create** per aggiungere il nuovo tipo di gateway.
10. Fai clic su **Next** per avviare il processo di aggiunta del tuo dispositivo gateway con il tipo di gateway selezionato.
11. Immetti l'ID dispositivo, come ad esempio `my_gateway_device`.
L'ID dispositivo viene utilizzato per identificare il dispositivo gateway nel dashboard {{site.data.keyword.iot_short_notm}} ed è anche un parametro obbligatorio per la connessione del tuo dispositivo gateway a {{site.data.keyword.iot_short_notm}}.
12. Facoltativo: fai clic su **Additional fields** per aggiungere informazioni sul dispositivo gateway, come il numero di serie, il produttore, il modello e così via.
 **Suggerimento:** puoi aggiungere e modificare le informazioni successivamente.
12. Facoltativo: immetti i metadati JSON del dispositivo.
 **Suggerimento:** puoi aggiungere e modificare i metadati del dispositivo successivamente. 
13. Fai clic su **Next** per completare l'aggiunta del tuo dispositivo gateway.
14. Verifica che le informazioni di riepilogo siano corrette e quindi fai clic su **Add** per aggiungere il dispositivo gateway.
**Suggerimento:** hai l'opzione di accettare un token di autenticazione generato automaticamente o di fornirne tu uno. Se scegli di creare il tuo proprio token, assicurati che sia compreso tra 8 e 36 caratteri, contenga un mix di caratteri maiuscoli e minuscoli, numeri, un trattino, un carattere di sottolineatura o un punto. Il token non deve contenere sequenze di caratteri ripetuti, parole del dizionario, nomi utente o altre sequenze predefinite.
15. Nella pagina delle informazioni del dispositivo, copia e salva le seguenti informazioni sul dispositivo:  
 - ID organizzazione, come ad esempio `tubo8x`
 - Tipo del dispositivo, come ad esempio `my_gateway_type`
 - ID del dispositivo. **Suggerimento:** per i dispositivi collegati alla rete, questo può essere ad esempio l'indirizzo MAC senza i due punti di separazione.
 - Metodo di autenticazione, come ad esempio `token`
 - Token di autenticazione, come ad esempio `PtBVriRqIg4uh)_-Kl`
  **Suggerimento:** hai bisogno dell'ID organizzazione, del token di autenticazione, del tipo di dispositivo e dell'ID del dispositivo per configurare il tuo dispositivo per il collegamento a {{site.data.keyword.iot_short_notm}}.  

Congratulazioni, hai registrato il tuo dispositivo gateway. Ora puoi configurare il tuo dispositivo gateway per collegarsi a {{site.data.keyword.iot_short_notm}}

## Passo 2: collegamento del tuo gateway a {{site.data.keyword.iot_short_notm}}
{: #connect_gateway}

Dopo aver registrato il gateway con {{site.data.keyword.iot_short_notm}} utilizza le informazioni registrate per collegare il gateway a {{site.data.keyword.iot_short_notm}} e inizia a ricevere i dati dai dispositivi collegati al gateway.

Per informazioni sulla connessione de tuo gateway a  {{site.data.keyword.iot_short_notm}}, consulta [Connettività MQTT per i gateway](mqtt.html).

**Suggerimento:** esiste una serie di ricette disponibili per il collegamento dei dispositivi a {{site.data.keyword.iot_short_notm}}. Per un elenco delle ricette, consulta
[Device connection recipes](https://developer.ibm.com/recipes/?post_type=tutorials&s=IoT) disponibili in IBM.com.


## Passo 3: collegamento dei dispositivi tramite il gateway
{: #gateway_devices}

I dispositivi collegati al gateway vengono automaticamente aggiunti a {{site.data.keyword.iot_short_notm}} quando il gateway pubblica i messaggi dal dispositivo. Vengono automaticamente creati il tipo di dispositivo e il dispositivo stesso quando il gateway pubblica un messaggio per una combinazione del tipo di dispositivo e dell'ID del dispositivo che non esiste ancora.

Per informazioni sulla registrazione e sulla pubblicazione automatica del dispositivo e la ricezione dei dati per i dispositivi collegati, consulta [Connettività MQTT per i gateway](mqtt.html).


Quando un dispositivo è stato collegato correttamente a un gateway, viene visualizzato sul dashboard della tua organizzazione {{site.data.keyword.iot_short_notm}}.

**Nota:** nel dashboard {{site.data.keyword.iot_short_notm}}, i dispositivi e i gateway collegati direttamente a {{site.data.keyword.iot_short_notm}} visualizzano un'icona di stato che indica che sono collegati. Il dashboard visualizza i dispositivi che sono collegati indirettamente tramite un gateway come scollegati perché non ha informazioni relative alla connettività dei dispositivi al gateway.


## Installazione di EAA (Edge Analytics Agent)
{: #edge}

EAA (Edge Analytics Agent) è un componente software creato con [Apache Quarks](http://quarks.incubator.apache.org/) per eseguire operazioni di analisi edge su un gateway caricando e gestendo le regole di analisi edge dal dashboard {{site.data.keyword.iot_short_notm}}. Per ulteriori informazioni sulle analisi edge, consulta [Analisi Edge](../edge_analytics.html).

### Installazione di EAA
{: #eaa_install}

Per installare EAA sul tuo gateway:
1. Nel dashboard {{site.data.keyword.iot_short}}, passa a **Rules**.
2. Fai clic su **Download Edge Agent** per andare alla [IBM Edge Analytics Agent community](https://www.ibm.com/developerworks/community/groups/service/html/communitystart?communityUuid=3df173af-0c21-4b9c-9fd1-e8e5561ef460&ftHelpTip=true).
3. Passa alla sezione **Files** e scarica il file *ibm-watson-iot-edge-analytics-dslink-java-0.0.1* compresso.
4. Per informazioni su come installare e configurare il componente software EAA nel tuo gateway, consulta la seguente ricetta: 
 - [Getting started with Edge Analytics in Watson IoT Platform](https://developer.ibm.com/recipes/?post_type=pnext_tutorial&p=19472)

### Impostazioni di configurazione EAA
{: #eaa_configuration}

Puoi utilizzare il file config.properties EAA per impostare i parametri di configurazione software di base.

Per aggiornare la configurazione EAA:
1. Nel sistema gateway in cui è in esecuzione EAA, individua il file config.properties EAA.
2. Prima di iniziare a modificare le impostazioni, crea una copia di backup del file.
3. Apri il file config.properties per la modifica.
4. Modifica i parametri di configurazione per il tuo ambiente:
 <dl>
 <dt>DataDirectSendEnable</dt>
 <dd>BOOLEAN (true|false)</br>
 TRUE (predefinito) - Invia tutti i dati a {{site.data.keyword.iot_short_notm}}.</br>
 FALSE - Invia solo i dati a {{site.data.keyword.iot_short_notm}} se sono impostate le regole nel motore. </dd>
 <dt>MonitorInterval</dt>
 <dd>INTEGER (millisecondi)</br>
 Il tempo in millisecondi prima che sia inviato un nuovo messaggio di monitoraggio a {{site.data.keyword.iot_short_notm}}. </br>
 Imposta un valore basso per notificare le metriche di monitoraggio più spesso. Imposta un valore elevato per avere informazioni di monitoraggio più dettagliate su {{site.data.keyword.iot_short_notm}}. </br>
 DEFAULT: 60000    </br>
 RECOMMENDED RANGE: [1000, 360000]</dd>
 <dt>MonitorLogDesample</dt>
 <dd>INTEGER  </br>
 Rapporto di decampionamento tra il numero di messaggi di monitoraggio inviati a {{site.data.keyword.iot_short_notm}} in confronto ai messaggi immessi nel logo locale. Ad esempio, se `MonitorLogDesample` è impostato su 10, solo una voce di log locale viene scritta ogni dieci messaggi che sono inviati a {{site.data.keyword.iot_short_notm}}. </br>Un numero grande mantiene il log locale piccolo. Un numero piccolo fornisce un log locale più dettagliato.</br>
 DEFAULT: 10</br>
 RECOMMENDED RANGE: [1, 100]</dd>
 <dt>MemoryAlertThreshold</dt>
 <dd>INTEGER (Megabyte)</br>
 La soglia di memoria heap JVM libera per cui viene inviato un messaggio di log di avvertenza di memoria al log delle diagnostiche di {{site.data.keyword.iot_short_notm}}. L'avviso è controllato dal monitoraggio. </br>Un valore basso riduce il numero di messaggi di avvio inviati a {{site.data.keyword.iot_short_notm}}. Un valore elevato ti fornisce un avviso più rapidamente se il server EAA sta riscontrando problemi di memoria.</br>**Suggerimento:** puoi utilizzare le regole di [analisi cloud](../cloud_analytics.html) per configurare le azioni di avviso come le notifiche email per inviarti avvisi sui problemi di memoria. Per informazioni sulle proprietà disponibili che puoi utilizzare per creare le regole, consulta [Metriche di diagnostica di EAA (Edge Analytics Agent)](../edge_analytics.html#eaa_metrics).</br>
  DEFAULT: 10</br>
 RECOMMENDED RANGE: [10 o 5% della memoria totale, 200]</dd>
 </dl>
5. Salva il file modificato e quindi riavvia EAA.

Esempio di file config.properties EAA
```
#######################################################################
# Engine Parameters
#######################################################################
# DataDirectSendEnable
#                    - BOOLEAN(true|false) Set to true to forward all
#                      the data; Set to false to disable data direct
#                      send to IOTP when there is no rule set in the
#                      engine.
#                      DEFAULT: true
#######################################################################
DataDirectSendEnable=true

#######################################################################
# Monitoring Parameters
#######################################################################
# MonitorInterval    - INTEGER Time interval in milliseconds before a  
#                      new monitoring message is generated. Set it to a
#                      small value to report the monitor metrics more
#                      frequently. Set it to a big value to have more
#                      detailed monitoring information on IoTP.
#                      DEFAULT: 60000    RECOMMEND: [1000, 360000]
# MonitorLogDesample - INTEGER The de-sampling ratio of the number of
#                      monitoring messages sent to IOTP vs. local log,
#                      i.e. number of monitoring messages N where
#                      EAA would output only one to the Info log for
#                      every N messages. Set it big to keep the size
#                      of the log small. Set it small to have detailed
#                      monitoring information in local log.
#                      DEFAULT: 10       RECOMMEND: [1, 100]
# MemoryAlertThreshold
#                    - INTEGER the free JVM heap memory in megabyte under
#                      which a log message would be generated and send to
#                      IOTP diagnosis log. The alert is driven by the
#                      monitoring. Set it small to eliminate unnecessary
#                      alert message on IoTP. Set it big to receive early
#                      alert when the system is likely to crash. Set to
#                      Min(Max(10MB, 5% of the total memory), 200MB) is
#                      recommended.
#                      DEFAULT: 10       RECOMMEND: [10, 200]
#######################################################################

MonitorInterval=60000
MonitorLogDesample=10
MemoryAlertThreshold=10
```
