---
title: Een webhook met een klassieke waarschuwing voor metrische gegevens in Azure Monitor aanroepen
description: Meer informatie over het omleiden van Azure-metrische waarschuwingen naar andere, niet-Azure-systemen.
author: snehithm
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 04/03/2017
ms.author: snmuvva
ms.subservice: alerts
ms.openlocfilehash: 88de4464e5b95b49e76e5d9c4f7dc0d6732076e1
ms.sourcegitcommit: e50a39eb97a0b52ce35fd7b1cf16c7a9091d5a2a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/21/2019
ms.locfileid: "74286165"
---
# <a name="call-a-webhook-with-a-classic-metric-alert-in-azure-monitor"></a>Een webhook met een klassieke waarschuwing voor metrische gegevens in Azure Monitor aanroepen
U kunt webhooks gebruiken om een Azure-waarschuwings melding naar andere systemen te routeren voor nabewerkingen of aangepaste acties. U kunt een webhook gebruiken voor een waarschuwing om deze te routeren naar services die SMS-berichten verzenden, fouten in het logboek registreren, een team informeren via chat-of bericht Services of voor verschillende andere acties. 

In dit artikel wordt beschreven hoe u een webhook instelt voor een waarschuwing voor een Azure-metriek. U ziet ook hoe de payload voor het HTTP POST-bericht op een webhook eruit ziet. Zie [een webhook aanroepen in een Azure-activiteiten logboek waarschuwing](alerts-log-webhook.md)voor meer informatie over de installatie en het schema voor een Azure-activiteiten logboek waarschuwing (waarschuwing voor gebeurtenissen).

Azure-waarschuwingen gebruiken HTTP POST om de waarschuwings inhoud in JSON-indeling te verzenden naar een webhook-URI die u opgeeft bij het maken van de waarschuwing. Het schema wordt verderop in dit artikel gedefinieerd. De URI moet een geldig HTTP-of HTTPS-eind punt zijn. Azure plaatst één vermelding per aanvraag wanneer een waarschuwing wordt geactiveerd.

## <a name="configure-webhooks-via-the-azure-portal"></a>Webhooks configureren via de Azure Portal
Als u de webhook-URI wilt toevoegen of bijwerken, gaat u in het [Azure Portal](https://portal.azure.com/)naar **waarschuwingen voor maken/bijwerken**.

![Een waarschuwings regel deel venster toevoegen](./media/alerts-webhooks/Alertwebhook.png)

U kunt ook een waarschuwing configureren voor het plaatsen van een bericht naar een webhook-URI door gebruik te maken van [Azure PowerShell-cmdlets](../../azure-monitor/platform/powershell-quickstart-samples.md#create-metric-alerts), een [cross-platform-cli](../../azure-monitor/platform/cli-samples.md#work-with-alerts)of [Azure monitor rest-api's](https://msdn.microsoft.com/library/azure/dn933805.aspx).

## <a name="authenticate-the-webhook"></a>De webhook verifiëren
De webhook kan worden geverifieerd met behulp van autorisatie op basis van tokens. De webhook-URI wordt opgeslagen met een token-ID. Bijvoorbeeld: `https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue`

## <a name="payload-schema"></a>Payload-schema
De POST-bewerking bevat de volgende JSON-nettolading en het schema voor alle waarschuwingen op basis van metrische gegevens:

```JSON
{
    "status": "Activated",
    "context": {
        "timestamp": "2015-08-14T22:26:41.9975398Z",
        "id": "/subscriptions/s1/resourceGroups/useast/providers/microsoft.insights/alertrules/ruleName1",
        "name": "ruleName1",
        "description": "some description",
        "conditionType": "Metric",
        "condition": {
            "metricName": "Requests",
            "metricUnit": "Count",
            "metricValue": "10",
            "threshold": "10",
            "windowSize": "15",
            "timeAggregation": "Average",
            "operator": "GreaterThanOrEqual"
        },
        "subscriptionId": "s1",
        "resourceGroupName": "useast",
        "resourceName": "mysite1",
        "resourceType": "microsoft.foo/sites",
        "resourceId": "/subscriptions/s1/resourceGroups/useast/providers/microsoft.foo/sites/mysite1",
        "resourceRegion": "centralus",
        "portalLink": "https://portal.azure.com/#resource/subscriptions/s1/resourceGroups/useast/providers/microsoft.foo/sites/mysite1"
    },
    "properties": {
        "key1": "value1",
        "key2": "value2"
    }
}
```


| Veld | Verplicht | Vaste set waarden | Opmerkingen |
|:--- |:--- |:--- |:--- |
| status |J |Geactiveerd, opgelost |De status van de waarschuwing op basis van de voor waarden die u hebt ingesteld. |
| context |J | |De context van de waarschuwing. |
| tijdstempel |J | |Het tijdstip waarop de waarschuwing is geactiveerd. |
| id |J | |Elke waarschuwings regel heeft een unieke ID. |
| name |J | |De naam van de waarschuwing. |
| description |J | |Een beschrijving van de waarschuwing. |
| conditionType |J |Metrische gegevens, gebeurtenis |Er worden twee soorten waarschuwingen ondersteund: metric en Event. Metrische waarschuwingen zijn gebaseerd op een metrische voor waarde. Gebeurtenis waarschuwingen zijn gebaseerd op een gebeurtenis in het activiteiten logboek. Gebruik deze waarde om te controleren of de waarschuwing is gebaseerd op een metrische waarde of een gebeurtenis. |
| condition |J | |De specifieke velden die moeten worden gecontroleerd op basis van de waarde van **conditionType** . |
| metricName |Voor metrische waarschuwingen | |De naam van de metriek die definieert wat de regel controleert. |
| metricUnit |Voor metrische waarschuwingen |Bytes, BytesPerSecond, Count, CountPerSecond, Percent, Seconds |De eenheid die is toegestaan in de metriek. Bekijk de [toegestane waarden](https://msdn.microsoft.com/library/microsoft.azure.insights.models.unit.aspx). |
| metricValue |Voor metrische waarschuwingen | |De werkelijke waarde van de metriek die de waarschuwing heeft veroorzaakt. |
| threshold |Voor metrische waarschuwingen | |De drempel waarde waarmee de waarschuwing wordt geactiveerd. |
| windowSize |Voor metrische waarschuwingen | |De periode die wordt gebruikt voor het bewaken van de waarschuwings activiteit op basis van de drempel waarde. De waarde moet tussen 5 minuten en 1 dag liggen. De waarde moet de ISO 8601-duur notatie hebben. |
| timeAggregation |Voor metrische waarschuwingen |Gemiddelde, laatste, maximum, minimum, geen, totaal |Hoe de verzamelde gegevens in de loop van de tijd moeten worden gecombineerd. De standaard waarde is gemiddelde. Bekijk de [toegestane waarden](https://msdn.microsoft.com/library/microsoft.azure.insights.models.aggregationtype.aspx). |
| and |Voor metrische waarschuwingen | |De operator die wordt gebruikt om de huidige metrische gegevens te vergelijken met de ingestelde drempel waarde. |
| subscriptionId |J | |De ID van het Azure-abonnement. |
| resourceGroupName |J | |De naam van de resource groep voor de betrokken resource. |
| resourceName |J | |De resource naam van de betrokken resource. |
| resourceType |J | |Het resource type van de betrokken resource. |
| resourceId |J | |De resource-ID van de betrokken resource. |
| resourceRegion |J | |De regio of locatie van de betrokken resource. |
| portalLink |J | |Een directe koppeling naar de overzichts pagina van de portal-resource. |
| properties |N |Optioneel |Een set sleutel-waardeparen met details over de gebeurtenis. Bijvoorbeeld `Dictionary<String, String>`. Het veld eigenschappen is optioneel. In een aangepaste UI-of logische werk stroom op basis van een app kunnen gebruikers sleutel-waardeparen invoeren die kunnen worden door gegeven via de payload. Een alternatieve manier om aangepaste eigenschappen door te geven aan de webhook is via de webhook-URI zelf (als query parameters). |

> [!NOTE]
> U kunt het **Eigenschappen** veld alleen instellen met behulp van [Azure monitor rest-api's](https://msdn.microsoft.com/library/azure/dn933805.aspx).
>
>

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over Azure-waarschuwingen en webhooks in de video [integreert Azure Alerts met PagerDuty](https://go.microsoft.com/fwlink/?LinkId=627080).
* Meer informatie over het [uitvoeren van Azure Automation scripts (runbooks) in azure-waarschuwingen](https://go.microsoft.com/fwlink/?LinkId=627081).
* Meer informatie over het [gebruik van een logische app voor het verzenden van een SMS-bericht via Twilio vanuit een Azure-waarschuwing](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app).
* Meer informatie over het [gebruik van een logische app voor het verzenden van een bericht met een toegestane vertraging van een Azure-waarschuwing](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app).
* Meer informatie over hoe u [een logische app kunt gebruiken om een bericht te verzenden naar een Azure-wachtrij vanuit een Azure-waarschuwing](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app).

