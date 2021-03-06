---
title: Gegevens fabrieken bewaken met behulp van Azure Monitor
description: Meer informatie over het gebruik van Azure Monitor om/Azure Data Factory-pijp lijnen te bewaken door Diagnostische logboeken in te scha kelen met informatie van Data Factory.
services: data-factory
documentationcenter: ''
author: djpmsft
ms.author: daperlov
manager: jroth
ms.reviewer: maghan
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 12/11/2018
ms.openlocfilehash: 67709ef96ffb8190812d625c04cd9749c0ebb900
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/06/2019
ms.locfileid: "73684616"
---
# <a name="alert-and-monitor-data-factories-by-using-azure-monitor"></a>Gegevens fabrieken melden en bewaken met behulp van Azure Monitor

Cloud toepassingen zijn complex en hebben veel bewegende onderdelen. Monitors bieden gegevens waarmee u ervoor kunt zorgen dat uw toepassingen in een goede staat blijven werken en worden uitgevoerd. Monitors helpen u om potentiële problemen te voor komen en eerdere problemen op te lossen.

U kunt bewakings gegevens gebruiken om uitgebreid inzicht te krijgen in uw toepassingen. Deze kennis helpt u bij het verbeteren van de prestaties en het onderhoud van toepassingen. Daarnaast kunt u hiermee acties automatiseren die anders hand matig moeten worden uitgevoerd.

Azure Monitor biedt metrische gegevens op basis van de infra structuur en logboeken voor de meeste Azure-Services. Diagnostische logboeken van Azure worden verzonden door een resource en bieden uitgebreide, frequente gegevens over de werking van die resource. En Azure Data Factory schrijft diagnostische Logboeken in de monitor.

Zie [Azure monitor Overview](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-azure-monitor)voor meer informatie.

## <a name="keeping-azure-data-factory-data"></a>Azure Data Factory gegevens bewaren

Data Factory Stores-pijp lijn-gegevens slechts 45 dagen worden uitgevoerd. Gebruik monitor als u deze gegevens gedurende een langere periode wilt houden. Met monitor kunt u Diagnostische logboeken routeren voor analyse. U kunt ze ook bewaren in een opslag account, zodat u de fabrieks gegevens hebt voor de gekozen duur.

## <a name="diagnostic-logs"></a>Diagnostische logboeken

* Sla de diagnostische logboeken op in een opslag account voor controle of hand matige inspectie. U kunt de diagnostische instellingen gebruiken om de Bewaar tijd in dagen op te geven.
* De logboeken streamen naar Azure Event Hubs. De logboeken worden ingevoerd in een partner service of op een aangepaste analyse oplossing, zoals Power BI.
* Analyseer de logboeken met Log Analytics.

U kunt een opslag account of een event hub-naam ruimte gebruiken die niet voor komt in het abonnement van de resource waarmee logboeken worden verzonden. De gebruiker die de instelling configureert, moet de juiste RBAC-toegang (op rollen gebaseerd toegangs beheer) hebben voor beide abonnementen.

## <a name="set-up-diagnostic-logs"></a>Diagnostische logboeken instellen

### <a name="diagnostic-settings"></a>Diagnostische instellingen

Diagnostische instellingen gebruiken voor het configureren van Diagnostische logboeken voor niet-reken resources. De instellingen voor een resource-besturings element hebben de volgende kenmerken:

* Ze geven aan waar Diagnostische logboeken worden verzonden. Voor beelden zijn onder andere een Azure-opslag account, een Azure Event Hub of controle Logboeken.
* Ze geven aan welke logboek categorieën worden verzonden.
* Ze geven op hoe lang elke logboek categorie moet worden bewaard in een opslag account.
* Een Bewaar periode van nul dagen houdt in dat Logboeken permanent worden bewaard. Anders kan de waarde een wille keurig aantal dagen van 1 tot en met 2.147.483.647 zijn.
* Als het Bewaar beleid is ingesteld, maar logboeken opslaat in een opslag account is uitgeschakeld, heeft het Bewaar beleid geen effect. Dit probleem kan bijvoorbeeld optreden wanneer alleen de opties Event Hubs of controle logboeken zijn geselecteerd.
* Bewaar beleid wordt per dag toegepast. De grens tussen dagen vindt plaats om middernacht Coordinated Universal Time (UTC). Aan het einde van de dag worden logboeken van dagen die het Bewaar beleid overschrijden, verwijderd. Als u bijvoorbeeld een Bewaar beleid van één dag hebt, wordt aan het begin van de huidige datum de logboeken van vóór gisteren verwijderd.

### <a name="enable-diagnostic-logs-via-the-azure-monitor-rest-api"></a>Diagnostische logboeken inschakelen via de Azure Monitor REST API

#### <a name="create-or-update-a-diagnostics-setting-in-the-monitor-rest-api"></a>Een diagnostische instelling in de monitor REST API maken of bijwerken

##### <a name="request"></a>Aanvraag

```
PUT
https://management.azure.com/{resource-id}/providers/microsoft.insights/diagnosticSettings/service?api-version={api-version}
```

##### <a name="headers"></a>Headers

* Vervang `{api-version}` door `2016-09-01`.
* Vervang `{resource-id}` door de ID van de resource waarvoor u de diagnostische instellingen wilt bewerken. Zie [Resourcegroepen gebruiken om Azure-resources te beheren](../azure-resource-manager/manage-resource-groups-portal.md) voor meer informatie.
* Stel de `Content-Type`-header in op `application/json`.
* Stel de autorisatie-header in op het JSON-webtoken dat u hebt ontvangen van Azure Active Directory (Azure AD). Zie [verificatie aanvragen](../active-directory/develop/authentication-scenarios.md)voor meer informatie.

##### <a name="body"></a>Hoofdtekst

```json
{
    "properties": {
        "storageAccountId": "/subscriptions/<subID>/resourceGroups/<resourceGroupName>/providers/Microsoft.Storage/storageAccounts/<storageAccountName>",
        "serviceBusRuleId": "/subscriptions/<subID>/resourceGroups/<resourceGroupName>/providers/Microsoft.EventHub/namespaces/<eventHubName>/authorizationrules/RootManageSharedAccessKey",
        "workspaceId": "/subscriptions/<subID>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<LogAnalyticsName>",
        "metrics": [
        ],
        "logs": [
                {
                    "category": "PipelineRuns",
                    "enabled": true,
                    "retentionPolicy": {
                        "enabled": false,
                        "days": 0
                    }
                },
                {
                    "category": "TriggerRuns",
                    "enabled": true,
                    "retentionPolicy": {
                        "enabled": false,
                        "days": 0
                    }
                },
                {
                    "category": "ActivityRuns",
                    "enabled": true,
                    "retentionPolicy": {
                        "enabled": false,
                        "days": 0
                    }
                }
            ]
    },
    "location": ""
}
```

| Eigenschap | Type | Beschrijving |
| --- | --- | --- |
| **storageAccountId** |Tekenreeks | De resource-ID van het opslag account waarnaar u Diagnostische logboeken wilt verzenden. |
| **Servicebusruleid kunnen** |Tekenreeks | De service bus-regel-ID van de service bus-naam ruimte waarin u Event Hubs wilt maken voor het streamen van Diagnostische logboeken. De regel-ID heeft de indeling `{service bus resource ID}/authorizationrules/{key name}`.|
| **workspaceId** | Complex type | Een matrix met metrische tijd korrels en het Bewaar beleid. De waarde van deze eigenschap is leeg. |
|**metrics**| Parameter waarden van de pijplijn uitvoering worden door gegeven aan de aangeroepen pijp lijn| Een JSON-object waarmee parameter namen worden toegewezen aan argument waarden. |
| **hout**| Complex type| De naam van een diagnostische logboek categorie voor een resource type. Als u de lijst met diagnostische logboek categorieën voor een resource wilt ophalen, voert u de bewerking Diagnostische instellingen ophalen uit. |
| **rubriek**| Tekenreeks| Een matrix met logboek categorieën en het Bewaar beleid. |
| **timeGrain** | Tekenreeks | De granulatie van metrische gegevens, die worden vastgelegd in de ISO 8601-duur notatie. De waarde van de eigenschap moet `PT1M`zijn, die één minuut aangeeft. |
| **ingeschakeld**| Booleaans | Hiermee wordt aangegeven of de verzameling van de categorie metrisch of logboek is ingeschakeld voor deze resource. |
| **retentionPolicy**| Complex type| Hierin wordt het Bewaar beleid voor een metrische of logboek categorie beschreven. Deze eigenschap wordt alleen gebruikt voor opslag accounts. |
|**resterende**| integer| Het aantal dagen dat de metrische gegevens of logboeken moeten worden bewaard. Als de waarde van de eigenschap 0 is, worden de logboeken permanent bewaard. Deze eigenschap wordt alleen gebruikt voor opslag accounts. |

##### <a name="response"></a>Antwoord

200 OK.


```json
{
    "id": "/subscriptions/<subID>/resourcegroups/adf/providers/microsoft.datafactory/factories/shloadobetest2/providers/microsoft.insights/diagnosticSettings/service",
    "type": null,
    "name": "service",
    "location": null,
    "kind": null,
    "tags": null,
    "properties": {
        "storageAccountId": "/subscriptions/<subID>/resourceGroups/<resourceGroupName>//providers/Microsoft.Storage/storageAccounts/<storageAccountName>",
        "serviceBusRuleId": "/subscriptions/<subID>/resourceGroups/<resourceGroupName>//providers/Microsoft.EventHub/namespaces/<eventHubName>/authorizationrules/RootManageSharedAccessKey",
        "workspaceId": "/subscriptions/<subID>/resourceGroups/<resourceGroupName>//providers/Microsoft.OperationalInsights/workspaces/<LogAnalyticsName>",
        "eventHubAuthorizationRuleId": null,
        "eventHubName": null,
        "metrics": [],
        "logs": [
            {
                "category": "PipelineRuns",
                "enabled": true,
                "retentionPolicy": {
                    "enabled": false,
                    "days": 0
                }
            },
            {
                "category": "TriggerRuns",
                "enabled": true,
                "retentionPolicy": {
                    "enabled": false,
                    "days": 0
                }
            },
            {
                "category": "ActivityRuns",
                "enabled": true,
                "retentionPolicy": {
                    "enabled": false,
                    "days": 0
                }
            }
        ]
    },
    "identity": null
}
```

#### <a name="get-information-about-diagnostics-settings-in-the-monitor-rest-api"></a>Informatie over diagnostische instellingen in de monitor REST API weer geven

##### <a name="request"></a>Aanvraag

```
GET
https://management.azure.com/{resource-id}/providers/microsoft.insights/diagnosticSettings/service?api-version={api-version}
```

##### <a name="headers"></a>Headers

* Vervang `{api-version}` door `2016-09-01`.
* Vervang `{resource-id}` door de ID van de resource waarvoor u de diagnostische instellingen wilt bewerken. Zie [Resourcegroepen gebruiken om Azure-resources te beheren](../azure-resource-manager/manage-resource-groups-portal.md) voor meer informatie.
* Stel de `Content-Type`-header in op `application/json`.
* Stel de autorisatie-header in op een JSON-webtoken dat u hebt ontvangen van Azure AD. Zie [verificatie aanvragen](../active-directory/develop/authentication-scenarios.md)voor meer informatie.

##### <a name="response"></a>Antwoord

200 OK.

```json
{
    "id": "/subscriptions/<subID>/resourcegroups/adf/providers/microsoft.datafactory/factories/shloadobetest2/providers/microsoft.insights/diagnosticSettings/service",
    "type": null,
    "name": "service",
    "location": null,
    "kind": null,
    "tags": null,
    "properties": {
        "storageAccountId": "/subscriptions/<subID>/resourceGroups/shloprivate/providers/Microsoft.Storage/storageAccounts/azmonlogs",
        "serviceBusRuleId": "/subscriptions/<subID>/resourceGroups/shloprivate/providers/Microsoft.EventHub/namespaces/shloeventhub/authorizationrules/RootManageSharedAccessKey",
        "workspaceId": "/subscriptions/<subID>/resourceGroups/ADF/providers/Microsoft.OperationalInsights/workspaces/mihaipie",
        "eventHubAuthorizationRuleId": null,
        "eventHubName": null,
        "metrics": [],
        "logs": [
            {
                "category": "PipelineRuns",
                "enabled": true,
                "retentionPolicy": {
                    "enabled": false,
                    "days": 0
                }
            },
            {
                "category": "TriggerRuns",
                "enabled": true,
                "retentionPolicy": {
                    "enabled": false,
                    "days": 0
                }
            },
            {
                "category": "ActivityRuns",
                "enabled": true,
                "retentionPolicy": {
                    "enabled": false,
                    "days": 0
                }
            }
        ]
    },
    "identity": null
}

```
Zie [Diagnostische instellingen](https://docs.microsoft.com/rest/api/monitor/diagnosticsettings)voor meer informatie.

## <a name="schema-of-logs-and-events"></a>Schema van Logboeken en gebeurtenissen

### <a name="monitor-schema"></a>Schema controleren

#### <a name="activity-run-log-attributes"></a>Activiteiten-logboek kenmerken uitvoeren

```json
{
   "Level": "",
   "correlationId":"",
   "time":"",
   "activityRunId":"",
   "pipelineRunId":"",
   "resourceId":"",
   "category":"ActivityRuns",
   "level":"Informational",
   "operationName":"",
   "pipelineName":"",
   "activityName":"",
   "start":"",
   "end":"",
   "properties":
       {
          "Input": "{
              "source": {
                "type": "BlobSource"
              },
              "sink": {
                "type": "BlobSink"
              }
           }",
          "Output": "{"dataRead":121,"dataWritten":121,"copyDuration":5,
               "throughput":0.0236328132,"errors":[]}",
          "Error": "{
              "errorCode": "null",
              "message": "null",
              "failureType": "null",
              "target": "CopyBlobtoBlob"
        }
    }
}
```

| Eigenschap | Type | Beschrijving | Voorbeeld |
| --- | --- | --- | --- |
| **Niveau** |Tekenreeks | Het niveau van de diagnostische Logboeken. Stel de waarde van de eigenschap in op 4 voor de logboeken voor de uitvoering van de activiteit. | `4` |
| **Correlatie** |Tekenreeks | De unieke ID voor het bijhouden van een bepaalde aanvraag. | `319dc6b4-f348-405e-b8d7-aafc77b73e77` |
| **tegelijk** | Tekenreeks | De tijd van de gebeurtenis in de UTC-notatie time span `YYYY-MM-DDTHH:MM:SS.00000Z`. | `2017-06-28T21:00:27.3534352Z` |
|**activityRunId**| Tekenreeks| De ID van de uitvoering van de activiteit. | `3a171e1f-b36e-4b80-8a54-5625394f4354` |
|**pipelineRunId**| Tekenreeks| De ID van de pijplijn uitvoering. | `9f6069d6-e522-4608-9f99-21807bfc3c70` |
|**resourceId**| Tekenreeks | De ID die is gekoppeld aan de Data Factory-resource. | `/SUBSCRIPTIONS/<subID>/RESOURCEGROUPS/<resourceGroupName>/PROVIDERS/MICROSOFT.DATAFACTORY/FACTORIES/<dataFactoryName>` |
|**rubriek**| Tekenreeks | De categorie van de diagnostische Logboeken. Stel de eigenschaps waarde in op `ActivityRuns`. | `ActivityRuns` |
|**afvlakking**| Tekenreeks | Het niveau van de diagnostische Logboeken. Stel de eigenschaps waarde in op `Informational`. | `Informational` |
|**operationName**| Tekenreeks | De naam van de activiteit met de status. Als de activiteit de heartbeat start is, wordt de waarde van de eigenschap `MyActivity -`. Als de activiteit de end heartbeat is, wordt de waarde van de eigenschap `MyActivity - Succeeded`. | `MyActivity - Succeeded` |
|**pipelineName**| Tekenreeks | De naam van de pijp lijn. | `MyPipeline` |
|**activityName**| Tekenreeks | De naam van de activiteit. | `MyActivity` |
|**start**| Tekenreeks | De begin tijd van de activiteit wordt uitgevoerd met de UTC-notatie time span. | `2017-06-26T20:55:29.5007959Z`|
|**endsystemen**| Tekenreeks | De eind tijd van de activiteit wordt uitgevoerd met de UTC-notatie time span. Als in het diagnostische logboek wordt aangegeven dat een activiteit is gestart, maar nog niet is beëindigd, wordt de waarde van de eigenschap `1601-01-01T00:00:00Z`. | `2017-06-26T20:55:29.5007959Z` |

#### <a name="pipeline-run-log-attributes"></a>Pijp lijn-kenmerken van Logboeken uitvoeren

```json
{
   "Level": "",
   "correlationId":"",
   "time":"",
   "runId":"",
   "resourceId":"",
   "category":"PipelineRuns",
   "level":"Informational",
   "operationName":"",
   "pipelineName":"",
   "start":"",
   "end":"",
   "status":"",
   "properties":
    {
      "Parameters": {
        "<parameter1Name>": "<parameter1Value>"
      },
      "SystemParameters": {
        "ExecutionStart": "",
        "TriggerId": "",
        "SubscriptionId": ""
      }
    }
}
```

| Eigenschap | Type | Beschrijving | Voorbeeld |
| --- | --- | --- | --- |
| **Niveau** |Tekenreeks | Het niveau van de diagnostische Logboeken. Stel de waarde van de eigenschap in op 4 voor de logboeken voor de uitvoering van de activiteit. | `4` |
| **Correlatie** |Tekenreeks | De unieke ID voor het bijhouden van een bepaalde aanvraag. | `319dc6b4-f348-405e-b8d7-aafc77b73e77` |
| **tegelijk** | Tekenreeks | De tijd van de gebeurtenis in de UTC-notatie time span `YYYY-MM-DDTHH:MM:SS.00000Z`. | `2017-06-28T21:00:27.3534352Z` |
|**runId**| Tekenreeks| De ID van de pijplijn uitvoering. | `9f6069d6-e522-4608-9f99-21807bfc3c70` |
|**resourceId**| Tekenreeks | De ID die is gekoppeld aan de Data Factory-resource. | `/SUBSCRIPTIONS/<subID>/RESOURCEGROUPS/<resourceGroupName>/PROVIDERS/MICROSOFT.DATAFACTORY/FACTORIES/<dataFactoryName>` |
|**rubriek**| Tekenreeks | De categorie van de diagnostische Logboeken. Stel de eigenschaps waarde in op `PipelineRuns`. | `PipelineRuns` |
|**afvlakking**| Tekenreeks | Het niveau van de diagnostische Logboeken. Stel de eigenschaps waarde in op `Informational`. | `Informational` |
|**operationName**| Tekenreeks | De naam van de pijp lijn samen met de status. Nadat de uitvoering van de pijp lijn is voltooid, wordt de waarde van de eigenschap `Pipeline - Succeeded`. | `MyPipeline - Succeeded`. |
|**pipelineName**| Tekenreeks | De naam van de pijp lijn. | `MyPipeline` |
|**start**| Tekenreeks | De begin tijd van de activiteit wordt uitgevoerd met de UTC-notatie time span. | `2017-06-26T20:55:29.5007959Z`. |
|**endsystemen**| Tekenreeks | De eind tijd van de activiteit wordt uitgevoerd met de UTC-notatie time span. Als in het diagnostische logboek wordt aangegeven dat een activiteit is gestart, maar nog niet is beëindigd, wordt de waarde van de eigenschap `1601-01-01T00:00:00Z`.  | `2017-06-26T20:55:29.5007959Z` |
|**hebben**| Tekenreeks | De laatste status van de pijplijn uitvoering. Mogelijke eigenschaps waarden zijn `Succeeded` en `Failed`. | `Succeeded`|

#### <a name="trigger-run-log-attributes"></a>Logboek kenmerken van de trigger-uitvoer

```json
{
   "Level": "",
   "correlationId":"",
   "time":"",
   "triggerId":"",
   "resourceId":"",
   "category":"TriggerRuns",
   "level":"Informational",
   "operationName":"",
   "triggerName":"",
   "triggerType":"",
   "triggerEvent":"",
   "start":"",
   "status":"",
   "properties":
   {
      "Parameters": {
        "TriggerTime": "",
       "ScheduleTime": ""
      },
      "SystemParameters": {}
    }
}

```

| Eigenschap | Type | Beschrijving | Voorbeeld |
| --- | --- | --- | --- |
| **Niveau** |Tekenreeks | Het niveau van de diagnostische Logboeken. Stel de waarde van de eigenschap in op 4 voor de logboeken voor de uitvoering van de activiteit. | `4` |
| **Correlatie** |Tekenreeks | De unieke ID voor het bijhouden van een bepaalde aanvraag. | `319dc6b4-f348-405e-b8d7-aafc77b73e77` |
| **tegelijk** | Tekenreeks | De tijd van de gebeurtenis in de UTC-notatie time span `YYYY-MM-DDTHH:MM:SS.00000Z`. | `2017-06-28T21:00:27.3534352Z` |
|**triggerId**| Tekenreeks| De ID van de trigger uitvoering. | `08587023010602533858661257311` |
|**resourceId**| Tekenreeks | De ID die is gekoppeld aan de Data Factory-resource. | `/SUBSCRIPTIONS/<subID>/RESOURCEGROUPS/<resourceGroupName>/PROVIDERS/MICROSOFT.DATAFACTORY/FACTORIES/<dataFactoryName>` |
|**rubriek**| Tekenreeks | De categorie van de diagnostische Logboeken. Stel de eigenschaps waarde in op `PipelineRuns`. | `PipelineRuns` |
|**afvlakking**| Tekenreeks | Het niveau van de diagnostische Logboeken. Stel de eigenschaps waarde in op `Informational`. | `Informational` |
|**operationName**| Tekenreeks | De naam van de trigger met de eind status, die aangeeft of de trigger is geactiveerd. Als de heartbeat is geslaagd, is de waarde van de eigenschap `MyTrigger - Succeeded`. | `MyTrigger - Succeeded` |
|**triggerName**| Tekenreeks | De naam van de trigger. | `MyTrigger` |
|**Trigger type**| Tekenreeks | Het type van de trigger. Mogelijke eigenschaps waarden zijn `Manual Trigger` en `Schedule Trigger`. | `ScheduleTrigger` |
|**triggerEvent**| Tekenreeks | Het gebeurtenis van de trigger. | `ScheduleTime - 2017-07-06T01:50:25Z` |
|**start**| Tekenreeks | De begin tijd van de trigger die wordt geactiveerd in timeas UTC-notatie. | `2017-06-26T20:55:29.5007959Z`|
|**hebben**| Tekenreeks | De uiteindelijke status die aangeeft of de trigger is geactiveerd. Mogelijke eigenschaps waarden zijn `Succeeded` en `Failed`. | `Succeeded`|

### <a name="log-analytics-schema"></a>Log Analytics schema

Log Analytics neemt het schema over van monitor met de volgende uitzonde ringen:

* De eerste letter in elke kolom naam wordt gekapitaliseerd. De kolom naam ' correlationId ' in de monitor is bijvoorbeeld ' CorrelationId ' in Log Analytics.
* Er is geen kolom van het niveau.
* De dynamische kolom ' Eigenschappen ' wordt bewaard als het volgende dynamische JSON-BLOB-type.

    | Azure Monitor kolom | Log Analytics kolom | Type |
    | --- | --- | --- |
    | $. Properties. UserProperties | UserProperties | Dynamisch |
    | $. Properties. Aantekeningen | Aantekeningen | Dynamisch |
    | $. Properties. Ingevoerd | Invoer | Dynamisch |
    | $. Properties. Uitvoer | Uitvoer | Dynamisch |
    | $. Properties. Fout. error code | Code | int |
    | $. Properties. Fout. Message | errorMessage | tekenreeks |
    | $. Properties. Optreedt | Fout | Dynamisch |
    | $. Properties. Voorgangers dikwijls werden | Voorgangers dikwijls werden | Dynamisch |
    | $. Properties. Instellen | Parameters | Dynamisch |
    | $. Properties. SystemParameters | SystemParameters | Dynamisch |
    | $. Properties. Koptags | Tags | Dynamisch |
    
## <a name="metrics"></a>Metrische gegevens

Met monitor kunt u inzicht krijgen in de prestaties en status van uw Azure-workloads. Het belangrijkste type monitor gegevens is de metriek. dit wordt ook wel het prestatie meter item genoemd. Metrische gegevens worden verzonden door de meeste Azure-resources. Monitor biedt verschillende manieren om deze metrische gegevens te configureren en te gebruiken voor bewaking en probleem oplossing.

Azure Data Factory versie 2 verzendt de volgende metrische gegevens.

| **Gegevens**           | **Weergave naam voor metrische gegevens**         | **Teleenheid** | **Aggregatie type** | **Beschrijving**                                       |
|----------------------|---------------------------------|----------|----------------------|-------------------------------------------------------|
| PipelineSucceededRuns | Metrische uitvoerings metingen geslaagde pijp lijnen | Count    | Totaal                | Het totale aantal uitvoeringen van de pijp lijn dat is geslaagd binnen een minuut venster. |
| PipelineFailedRuns   | Metrische gegevens van mislukte pijplijn uitvoeringen    | Count    | Totaal                | Het totale aantal uitvoeringen van de pijp lijn dat is mislukt binnen een minuut venster.    |
| ActivitySucceededRuns | Metrische gegevens uitvoeringen uitgevoerde activiteit | Count    | Totaal                | Het totale aantal uitgevoerde activiteiten in een minuut venster.  |
| ActivityFailedRuns   | Metrische gegevens mislukte uitvoering van activiteit    | Count    | Totaal                | Het totale aantal uitgevoerde activiteiten in een minuut venster.     |
| TriggerSucceededRuns | Meet waarden voor uitvoering van geslaagde triggers  | Count    | Totaal                | Het totale aantal uitvoeringen van triggers dat is geslaagd binnen een minuut venster.   |
| TriggerFailedRuns    | Meet waarden voor uitvoering van mislukte triggers     | Count    | Totaal                | Het totale aantal uitvoeringen van triggers dat is mislukt binnen een minuut venster.      |

Volg de instructies in [Azure monitor data platform](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-metrics)om toegang te krijgen tot de metrische gegevens.

## <a name="monitor-data-factory-metrics-with-azure-monitor"></a>Data Factory metrische gegevens bewaken met Azure Monitor

U kunt Data Factory integratie met monitor gebruiken om gegevens te routeren om te bewaken. Deze integratie is handig in de volgende scenario's:

* U wilt complexe query's schrijven op een uitgebreide set metrische gegevens die worden gepubliceerd door Data Factory om te bewaken. U kunt met behulp van monitor aangepaste waarschuwingen maken voor deze query's.

* U wilt controleren op gegevens fabrieken. U kunt gegevens van meerdere gegevens fabrieken naar een enkele werk ruimte van de monitor routeren.

Bekijk de volgende video voor een inleiding en demonstratie van zeven minuten voor deze functie:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Monitor-Data-Factory-pipelines-using-Operations-Management-Suite-OMS/player]

### <a name="configure-diagnostic-settings-and-workspace"></a>Diagnostische instellingen en werk ruimte configureren

Diagnostische instellingen voor uw data factory maken of toevoegen.

1. Ga in de portal naar monitor. Selecteer **instellingen** > **Diagnostische instellingen**.

1. Selecteer de data factory waarvoor u een diagnostische instelling wilt instellen.

1. Als er geen instellingen zijn op de geselecteerde data factory, wordt u gevraagd een instelling te maken. Selecteer **Diagnostische gegevens inschakelen**.

   ![Een diagnostische instelling maken als er geen instellingen zijn](media/data-factory-monitor-oms/monitor-oms-image1.png)

   Als er bestaande instellingen zijn op de data factory, ziet u een lijst met instellingen die al zijn geconfigureerd op de data factory. Selecteer **Diagnostische instelling toevoegen**.

   ![Een diagnostische instelling toevoegen als er instellingen bestaan](media/data-factory-monitor-oms/add-diagnostic-setting.png)

1. Geef een naam op voor de instelling, selecteer **verzenden naar log Analytics**en selecteer een werk ruimte in **log Analytics werk ruimte**.

    ![Geef uw instellingen een naam en selecteer een werk ruimte voor logboek analyse](media/data-factory-monitor-oms/monitor-oms-image2.png)

1. Selecteer **Opslaan**.

Na enkele ogen blikken wordt de nieuwe instelling weer gegeven in de lijst met instellingen voor deze data factory. Diagnostische logboeken worden naar die werk ruimte gestreamd zodra er nieuwe gebeurtenis gegevens worden gegenereerd. Maxi maal 15 minuten kan verstrijken tussen het moment dat een gebeurtenis wordt verzonden en wanneer deze wordt weer gegeven in Log Analytics.

* In de _resource-specifieke_ modus, Diagnostische logboeken van Azure Data Factory flow in _ADFPipelineRun_-, _ADFTriggerRun_-en _ADFActivityRun_ -tabellen
* In de _diagnostische modus van Azure_ worden diagnostische logboeken geplaatst in de tabel _AzureDiagnostics_

> [!NOTE]
> Omdat een Azure-logboek tabel niet meer dan 500 kolommen kan bevatten, raden we u ten zeerste aan de resource-specifieke modus te selecteren. Zie [log Analytics bekende beperkingen](../azure-monitor/platform/resource-logs-collect-workspace.md#column-limit-in-azurediagnostics)voor meer informatie.

### <a name="install-azure-data-factory-analytics-from-azure-marketplace"></a>Azure Data Factory Analytics installeren vanuit Azure Marketplace

![Ga naar ' Azure Marketplace ', voer ' Analytics filter ' in en selecteer ' Azure Data Factory Analytics (preview).](media/data-factory-monitor-oms/monitor-oms-image3.png)

![Meer informatie over ' Azure Data Factory Analytics (preview) '](media/data-factory-monitor-oms/monitor-oms-image4.png)

Selecteer **maken** en selecteer vervolgens **OMS-werk ruimte** en **OMS-werkruimte instellingen**.

![Een nieuwe oplossing maken](media/data-factory-monitor-oms/monitor-oms-image5.png)

### <a name="monitor-data-factory-metrics"></a>Data Factory metrische gegevens bewaken

Als u Azure Data Factory Analytics installeert, wordt er een standaardset weer gaven gemaakt zodat de volgende metrische gegevens worden ingeschakeld:

- ADF-uitvoeringen-1) pijplijn uitvoeringen door Data Factory
 
- ADF-uitvoeringen-2) uitvoering van activiteit door Data Factory

- ADF-uitvoeringen-3) activering wordt uitgevoerd door Data Factory

- ADF-fouten-1) Top 10 van pijplijn fouten per Data Factory

- ADF-fouten-2) de tien uitvoering van de activiteit wordt uitgevoerd door Data Factory

- ADF-fouten-3) de tien belangrijkste trigger fouten door Data Factory

- ADF-statistieken-1) uitvoering van activiteit per type

- ADF-statistieken-2) trigger uitvoeringen per type

- ADF-statistieken-3) maximale duur van pijplijn uitvoeringen

![Venster met ' werkmappen (preview) ' en ' AzureDataFactoryAnalytics ' gemarkeerd](media/data-factory-monitor-oms/monitor-oms-image6.png)

U kunt de voor gaande metrische gegevens visualiseren, de query's achter deze metrische gegevens bekijken, de query's bewerken, waarschuwingen maken en andere acties uitvoeren.

![Grafische weer gave van de pijp lijn wordt uitgevoerd door data factory "](media/data-factory-monitor-oms/monitor-oms-image8.png)

> [!NOTE]
> Met Azure Data Factory Analytics (preview) worden Diagnostische logboeken naar _resource-specifieke_ doel tabellen verzonden. U kunt query's schrijven op basis van de volgende tabellen: _ADFPipelineRun_, _ADFTriggerRun_en _ADFActivityRun_.

## <a name="alerts"></a>Waarschuwingen

Meld u aan bij de Azure Portal en selecteer > **waarschuwingen** **controleren** om waarschuwingen te maken.

![Waarschuwingen in het menu van de portal](media/monitor-using-azure-monitor/alerts_image3.png)

### <a name="create-alerts"></a>Waarschuwingen maken

1. Selecteer **+ nieuwe waarschuwings regel** om een nieuwe waarschuwing te maken.

    ![Nieuwe waarschuwings regel](media/monitor-using-azure-monitor/alerts_image4.png)

1. Definieer de waarschuwings voorwaarde.

    > [!NOTE]
    > Zorg ervoor dat u **alle** selecteert in de vervolg keuzelijst **filteren op resource type** .

    !["Waarschuwings voorwaarde definiëren" > "doel selecteren", waarmee het deel venster "een resource selecteren" wordt geopend ](media/monitor-using-azure-monitor/alerts_image5.png)

    ![' Geef een waarschuwings voorwaarde op ' > ' criteria toevoegen ', waarmee het deel venster ' signalerings logica configureren ' wordt geopend](media/monitor-using-azure-monitor/alerts_image6.png)

    ![Het deel venster signaal type configureren](media/monitor-using-azure-monitor/alerts_image7.png)

1. Definieer de details van de waarschuwing.

    ![Meldingsdetails](media/monitor-using-azure-monitor/alerts_image8.png)

1. Definieer de actie groep.

    ![Een regel maken, waarbij ' nieuwe actie groep ' is gemarkeerd](media/monitor-using-azure-monitor/alerts_image9.png)

    ![Een nieuwe actie groep maken](media/monitor-using-azure-monitor/alerts_image10.png)

    ![E-mail, SMS, push en Voice configureren](media/monitor-using-azure-monitor/alerts_image11.png)

    ![Een actie groep definiëren](media/monitor-using-azure-monitor/alerts_image12.png)

## <a name="next-steps"></a>Volgende stappen
[Pijp lijnen programmatisch controleren en beheren](monitor-programmatically.md)
