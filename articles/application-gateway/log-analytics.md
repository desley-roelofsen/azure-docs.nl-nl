---
title: WAF-logboeken onderzoeken met behulp van Azure Log Analytics
titleSuffix: Azure Application Gateway
description: In dit artikel wordt beschreven hoe u Azure Log Analytics kunt gebruiken om Application Gateway Web Application firewall-logboeken te onderzoeken
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 11/14/2019
ms.author: victorh
ms.openlocfilehash: 9fe4462a71852e5f66268f798f6f0418f2dd39c4
ms.sourcegitcommit: b1a8f3ab79c605684336c6e9a45ef2334200844b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/13/2019
ms.locfileid: "74048103"
---
# <a name="use-log-analytics-to-examine-application-gateway-web-application-firewall-logs"></a>Logboekanalyse gebruiken om Web Application Firewall-logboeken van Application Gateway te onderzoeken

Zodra uw Application Gateway WAF operationeel is, kunt u Logboeken inschakelen om te controleren wat er met elke aanvraag gebeurt. Firewall logboeken bieden inzicht in wat de WAF is te evalueren, vergelijken en blok keren. Met Log Analytics kunt u de gegevens in de logboeken van de firewall bekijken om nog meer inzichten te geven. Zie [een log Analytics-werk ruimte maken in de Azure Portal](../azure-monitor/learn/quick-create-workspace.md)voor meer informatie over het maken van een log Analytics-werk ruimte. Zie [overzicht van logboek query's in azure monitor](../azure-monitor/log-query/log-query-overview.md)voor meer informatie over logboek query's.

## <a name="import-waf-logs"></a>WAF-logboeken importeren

Als u uw firewall-logboeken wilt importeren in Log Analytics, raadpleegt u de [back-end-status, Diagnostische logboeken en metrische gegevens voor Application Gateway](application-gateway-diagnostics.md#diagnostic-logging). Wanneer u de firewall Logboeken in uw Log Analytics-werk ruimte hebt, kunt u gegevens weer geven, query's schrijven, visualisaties maken en deze toevoegen aan uw portal-dash board.

## <a name="explore-data-with-examples"></a>Gegevens verkennen met voor beelden

Als u de onbewerkte gegevens in het firewall logboek wilt weer geven, kunt u de volgende query uitvoeren:

```
AzureDiagnostics 
| where ResourceProvider == "MICROSOFT.NETWORK" and Category == "ApplicationGatewayFirewallLog"
```

Dit ziet er ongeveer uit als de volgende query:

![Log Analytics query](media/log-analytics/log-query.png)

U kunt inzoomen op de gegevens en grafieken tekenen of visualisaties maken. Bekijk de volgende query's als uitgangs punt:

### <a name="matchedblocked-requests-by-ip"></a>Overeenkomende/geblokkeerde aanvragen per IP

```
AzureDiagnostics
| where ResourceProvider == "MICROSOFT.NETWORK" and Category == "ApplicationGatewayFirewallLog"
| summarize count() by clientIp_s, bin(TimeGenerated, 1m)
| render timechart
```

### <a name="matchedblocked-requests-by-uri"></a>Overeenkomende/geblokkeerde aanvragen per URI

```
AzureDiagnostics
| where ResourceProvider == "MICROSOFT.NETWORK" and Category == "ApplicationGatewayFirewallLog"
| summarize count() by requestUri_s, bin(TimeGenerated, 1m)
| render timechart
```

### <a name="top-matched-rules"></a>Bovenaan overeenkomende regels

```
AzureDiagnostics
| where ResourceProvider == "MICROSOFT.NETWORK" and Category == "ApplicationGatewayFirewallLog"
| summarize count() by ruleId_s, bin(TimeGenerated, 1m)
| where count_ > 10
| render timechart
```

### <a name="top-five-matched-rule-groups"></a>Bovenste vijf overeenkomende regel groepen

```
AzureDiagnostics
| where ResourceProvider == "MICROSOFT.NETWORK" and Category == "ApplicationGatewayFirewallLog"
| summarize Count=count() by details_file_s, action_s
| top 5 by Count desc
| render piechart
```

## <a name="add-to-your-dashboard"></a>Toevoegen aan uw dash board

Wanneer u een query hebt gemaakt, kunt u deze toevoegen aan uw dash board.  Selecteer in de rechter bovenhoek van de log Analytics-werk ruimte de optie **vastmaken aan dash board** . Met de vorige vier query's die zijn vastgemaakt aan een voorbeeld dashboard, zijn dit de gegevens die u in een oogopslag kunt zien:

![Dashboard](media/log-analytics/dashboard.png)

## <a name="next-steps"></a>Volgende stappen

[Back-end-status, Diagnostische logboeken en metrische gegevens voor Application Gateway](application-gateway-diagnostics.md)
