---

copyright:
  years: 2015, 2015


---


{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:new_window: target="_blank"}  
{:shortdesc: .shortdesc}


# Problemas gerais de serviços
{: #general}

*Última atualização: 12 de agosto de 2016*
{: .last-updated}

Problemas de serviços do {{site.data.keyword.Bluemix}}
podem incluir um erro de tempo limite do gateway que ocorre quando você
exclui uma instância de serviço. No entanto, em vários casos, é possível recuperar-se desses
problemas seguindo algumas etapas simples.
{:shortdesc}

## Ocorre um erro de broker de serviço ao excluir uma instância de serviço
{: #ts_service_broker}

Você poderá receber uma mensagem de erro quando tentar excluir
uma instância de serviço que já foi excluída do controlador de nuvem.
{:shortdesc}


Ao tentar excluir uma instância de serviço, você verá a mensagem de erro do broker de serviço, ```Tempo limite do gateway```.
{: tsSymptoms}


Esse problema ocorrerá se
    a instância de serviço já tiver sido excluída do
controlador de nuvem.
{: tsCauses}


Para
    resolver esse problema, crie uma instância de serviço com o mesmo
nome de serviço e depois ligue-o aos seus aplicativos. Depois disso,
será possível excluir a instância de serviço e os aplicativos que usam
    o serviço.   
{: tsResolve}
