---

copyright:
  years: 2016

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}


# Modalità offline per Liberty
{: #offline_mode}
Ultimo aggiornamento: 20 luglio 2016
{: .last-updated}

Quando viene eseguito il push di un'applicazione Liberty a {{site.data.keyword.Bluemix}} il pacchetto di build Liberty può accedere ai siti esterni a Bluemix
per acquisire le risorse richieste dall'applicazione.  I seguenti sono dei siti esterni a cui può accedere il pacchetto di build Liberty.  Negli ambienti [Bluemix dedicato](../../dedicated/index.html#dedicated) e
[Bluemix locale](../../local/index.html#local) è necessario che questi siti siano *consentiti*.

* https://download.run.pivotal.io e https://java-buildpack.cloudfoundry.org sono utilizzati per accedere ai componenti di:
  * [AppDynamics agent](https://www.appdynamics.com/)
  * [MariaDB JDBC driver](https://mariadb.com/)
  * [New Relic agent](newRelic.html)
  * [OpenJDK ](customizingJRE.html#OpenJDK)
  * [PostgreSQL JDBC driver](https://www.postgresql.org)
* https://dl.zeroturnaround.com/jrebel/ è utilizzato per accedere ai componenti di [JRebel](https://zeroturnaround.com/software/jrebel/).
* https://download.ruxit.com/agent/paas/cloudfoundry/java è utilizzato per accedere ai componenti di [Dynatrace Ruxit agent](dynatrace.html).
* http://downloads.dynatracesaas.com/cloudfoundry/buildpack/java/  è utilizzato per accedere a [Dynatrace agent](dynatrace.html).

## Gestione di un proxy
{: #working_with_proxy}

In alcuni ambienti come [Bluemix dedicato](../../dedicated/index.html#dedicated) e
[Bluemix locale](../../local/index.html#local) può essere configurato un proxy. Consulta
[Gestione di un proxy](../../manageapps/workingWithProxy.html) per ulteriori dettagli.

# rellinks
{: #rellinks}
## general
{: #general}
* [Runtime Liberty](index.html)
* [Panoramica di Liberty Profile](http://www-01.ibm.com/support/knowledgecenter/SSAW57_8.5.5/com.ibm.websphere.wlp.nd.doc/ae/cwlp_about.html)
