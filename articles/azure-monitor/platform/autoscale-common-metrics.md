---
title: Algemene metrische gegevens automatisch schalen
description: Meer informatie over de metrische gegevens die vaak worden gebruikt voor het automatisch schalen van uw Cloud Services, Virtual Machines en Web Apps.
author: anirudhcavale
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 12/6/2016
ms.author: ancav
ms.subservice: autoscale
ms.openlocfilehash: 9da8e5fb88ff34e561b579b760973ecd23c884a3
ms.sourcegitcommit: 13d5eb9657adf1c69cc8df12486470e66361224e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/31/2019
ms.locfileid: "66129730"
---
# <a name="azure-monitor-autoscaling-common-metrics"></a>Azure Monitor automatisch schalen van algemene metrische gegevens

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Azure Monitor automatisch schalen, kunt u het aantal actieve instanties omhoog of omlaag schalen op basis van telemetriegegevens (metrische gegevens). In dit document worden algemene metrische gegevens beschreven die u mogelijk wilt gebruiken. In het Azure Portal kunt u de metriek kiezen van de resource die u wilt schalen. U kunt echter ook een wille keurige metriek kiezen van een andere resource om door te schalen.

Azure Monitor automatisch schalen is alleen van toepassing op [Virtual Machine Scale sets](https://azure.microsoft.com/services/virtual-machine-scale-sets/), [Cloud Services](https://azure.microsoft.com/services/cloud-services/), [app service-Web apps](https://azure.microsoft.com/services/app-service/web/)en [API Management Services](https://docs.microsoft.com/azure/api-management/api-management-key-concepts). Andere Azure-Services gebruiken verschillende schaal methoden.

## <a name="compute-metrics-for-resource-manager-based-vms"></a>Reken gegevens voor virtuele machines op basis van Resource Manager
Standaard worden metrische gegevens op basis van Resource Manager Virtual Machines en Virtual Machine Scale Sets basis waarden (op het niveau van de host). Wanneer u de diagnostische gegevens verzameling voor een Azure-VM en VMSS configureert, worden de diagnostische Azure-extensie bovendien ook prestatie meter items van het gast besturingssysteem (meestal ' metrische gegevens van het besturings systeem ' genoemd).  U gebruikt al deze metrische gegevens in regels voor automatisch schalen.

U kunt de `Get MetricDefinitions` API/PoSH/CLI gebruiken om de metrische gegevens weer te geven die beschikbaar zijn voor uw VMSS-resource.

Als u VM-schaal sets gebruikt en er geen specifieke metriek wordt weer gegeven, is deze waarschijnlijk *uitgeschakeld* in de uitbrei ding voor diagnostische gegevens.

Als een bepaalde metrische waarde niet wordt bemonsterd of overgedragen op de gewenste frequentie, kunt u de diagnostische configuratie bijwerken.

Als een van beide voor waarden waar is, raadpleegt u [Power shell gebruiken om Azure Diagnostics in te scha kelen op een virtuele machine met Windows](../../virtual-machines/extensions/diagnostics-windows.md) over Power shell om uw Azure VM Diagnostics-extensie te configureren en bij te werken om de metriek in te scha kelen. Dit artikel bevat ook een voor beeld van een configuratie bestand voor diagnostische gegevens.

### <a name="host-metrics-for-resource-manager-based-windows-and-linux-vms"></a>Metrische gegevens van de host voor Windows-en Linux-Vm's op basis van Resource Manager
De volgende metrische gegevens op hostniveau worden standaard verzonden voor Azure VM en VMSS in zowel Windows-als Linux-exemplaren. Deze metrische gegevens beschrijven uw Azure VM, maar worden verzameld van de Azure VM-host, in plaats van via een agent die is geïnstalleerd op de gast-VM. U kunt deze metrische gegevens gebruiken in regels voor automatisch schalen.

- [Metrische gegevens van de host voor Windows-en Linux-Vm's op basis van Resource Manager](../../azure-monitor/platform/metrics-supported.md#microsoftcomputevirtualmachines)
- [Metrische gegevens van de host voor Windows-en Linux-VM Scale Sets op basis van Resource Manager](../../azure-monitor/platform/metrics-supported.md#microsoftcomputevirtualmachinescalesets)

### <a name="guest-os-metrics-resource-manager-based-windows-vms"></a>Metrische Windows-Vm's op basis van een gast besturingssysteem (op Resource Manager gebaseerde)
Wanneer u een virtuele machine in azure maakt, wordt Diagnostische gegevens ingeschakeld met behulp van de diagnostische extensie. Met de uitbrei ding voor diagnostische gegevens wordt een set metrische gegevens verzonden die zijn gemaakt van binnen van de virtuele machine. Dit betekent dat u automatisch de metrische gegevens die niet standaard worden verzonden, kunt uitschalen.

U kunt een lijst met metrische gegevens genereren met behulp van de volgende opdracht in Power shell.

```
Get-AzMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

U kunt een waarschuwing maken voor de volgende metrische gegevens:

| Naam van meetwaarde | Eenheid |
| --- | --- |
| \Processor(_Total)\% Processor Time |Percent |
| \Processor (_Totaal)\% beschermde tijd |Percent |
| \Processor (_Totaal)\% tijd van gebruiker |Percent |
| \Processor Information (_Totaal) \Processor frequentie |Count |
| \System\Processes |Count |
| \Process(_Total)\Thread Count |Count |
| \Process(_Total)\Handle Count |Count |
| \Memory\% toegewezen bytes in gebruik |Percent |
| \Memory\Available Bytes |Bytes |
| \Memory\Committed bytes |Bytes |
| Limiet voor \Memory\Commit |Bytes |
| \Memory\Pool Paged Bytes |Bytes |
| Niet-wisselbaar \Memory\Pool bytes |Bytes |
| \PhysicalDisk (_Totaal)\% schijf tijd |Percent |
| \PhysicalDisk (_Totaal)\% Lees tijd schijf |Percent |
| \PhysicalDisk (_Totaal)\% schrijf tijd schijf |Percent |
| \PhysicalDisk(_Total)\Disk Transfers/sec |CountPerSecond |
| \PhysicalDisk(_Total)\Disk Reads/sec |CountPerSecond |
| \PhysicalDisk(_Total)\Disk Writes/sec |CountPerSecond |
| \PhysicalDisk(_Total)\Disk Bytes/sec |BytesPerSecond |
| \PhysicalDisk (_Totaal) \Gelezen Lees bewerkingen in bytes per seconde |BytesPerSecond |
| \PhysicalDisk(_Total)\Disk Write Bytes/sec |BytesPerSecond |
| \PhysicalDisk(_Total)\Avg. Lengte van de wachtrij voor de schijf |Count |
| \PhysicalDisk(_Total)\Avg. Wachtrij lengte voor lezen van schijf |Count |
| \PhysicalDisk(_Total)\Avg. Wachtrij lengte schrijf bewerking schijf |Count |
| \LogicalDisk (_Totaal)\% beschik bare ruimte |Percent |
| \LogicalDisk(_Total)\Free Megabytes |Count |

### <a name="guest-os-metrics-linux-vms"></a>Gegevens van virtuele machines van het gast besturingssysteem Linux-Vm's
Wanneer u een virtuele machine in azure maakt, wordt de diagnostische gegevens standaard ingeschakeld met behulp van de diagnostische extensie.

U kunt een lijst met metrische gegevens genereren met behulp van de volgende opdracht in Power shell.

```
Get-AzMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

 U kunt een waarschuwing maken voor de volgende metrische gegevens:

| Naam van meetwaarde | Eenheid |
| --- | --- |
| \Memory\AvailableMemory |Bytes |
| \Memory\PercentAvailableMemory |Percent |
| \Memory\UsedMemory |Bytes |
| \Memory\PercentUsedMemory |Percent |
| \Memory\PercentUsedByCache |Percent |
| \Memory\PagesPerSec |CountPerSecond |
| \Memory\PagesReadPerSec |CountPerSecond |
| \Memory\PagesWrittenPerSec |CountPerSecond |
| \Memory\AvailableSwap |Bytes |
| \Memory\PercentAvailableSwap |Percent |
| \Memory\UsedSwap |Bytes |
| \Memory\PercentUsedSwap |Percent |
| \Processor\PercentIdleTime |Percent |
| \Processor\PercentUserTime |Percent |
| \Processor\PercentNiceTime |Percent |
| \Processor\PercentPrivilegedTime |Percent |
| \Processor\PercentInterruptTime |Percent |
| \Processor\PercentDPCTime |Percent |
| \Processor\PercentProcessorTime |Percent |
| \Processor\PercentIOWaitTime |Percent |
| \PhysicalDisk\BytesPerSecond |BytesPerSecond |
| \PhysicalDisk\ReadBytesPerSecond |BytesPerSecond |
| \PhysicalDisk\WriteBytesPerSecond |BytesPerSecond |
| \PhysicalDisk\TransfersPerSecond |CountPerSecond |
| \PhysicalDisk\ReadsPerSecond |CountPerSecond |
| \PhysicalDisk\WritesPerSecond |CountPerSecond |
| \PhysicalDisk\AverageReadTime |Seconden |
| \PhysicalDisk\AverageWriteTime |Seconden |
| \PhysicalDisk\AverageTransferTime |Seconden |
| \PhysicalDisk\AverageDiskQueueLength |Count |
| \NetworkInterface\BytesTransmitted |Bytes |
| \NetworkInterface\BytesReceived |Bytes |
| \NetworkInterface\PacketsTransmitted |Count |
| \NetworkInterface\PacketsReceived |Count |
| \NetworkInterface\BytesTotal |Bytes |
| \NetworkInterface\TotalRxErrors |Count |
| \NetworkInterface\TotalTxErrors |Count |
| \NetworkInterface\TotalCollisions |Count |

## <a name="commonly-used-web-server-farm-metrics"></a>Veelgebruikte metrische gegevens voor Web (server-farm)
U kunt ook automatisch schalen uitvoeren op basis van algemene gegevens van webservers, zoals de lengte van de http-wachtrij. De naam van de metrische waarde is **HttpQueueLength**.  In de volgende sectie worden metrische gegevens van de beschik bare server farm (Web Apps) weer gegeven.

### <a name="web-apps-metrics"></a>Web Apps metrische gegevens
U kunt een lijst met Web Apps metrische gegevens genereren met behulp van de volgende opdracht in Power shell.

```
Get-AzMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

U kunt een melding ontvangen over of schalen op basis van deze metrische gegevens.

| Naam van meetwaarde | Eenheid |
| --- | --- |
| CpuPercentage |Percent |
| MemoryPercentage |Percent |
| DiskQueueLength |Count |
| HttpQueueLength |Count |
| BytesReceived |Bytes |
| BytesSent |Bytes |

## <a name="commonly-used-storage-metrics"></a>Veelgebruikte metrische opslag gegevens
U kunt schalen op de lengte van de opslag wachtrij. Dit is het aantal berichten in de opslag wachtrij. De lengte van de opslag wachtrij is een speciale metrische waarde en de drempel waarde is het aantal berichten per exemplaar. Als er bijvoorbeeld twee exemplaren zijn en als de drempel waarde is ingesteld op 100, wordt er geschaald wanneer het totale aantal berichten in de wachtrij 200 is. Dit kan 100 berichten per instantie, 120 en 80 of een andere combi natie van Maxi maal 200 of meer zijn.

Configureer deze instelling in de Azure Portal op de Blade **instellingen** . Voor VM-schaal sets kunt u de instelling voor automatisch schalen in het Resource Manager-sjabloon bijwerken om *metrische waarde* als *ApproximateMessageCount* te gebruiken en de id van de opslag wachtrij als *metricResourceUri*door te geven.

Met een klassiek opslag account kunt u bijvoorbeeld de instelling voor automatisch schalen metricTrigger:

```
"metricName": "ApproximateMessageCount",
 "metricNamespace": "",
 "metricResourceUri": "/subscriptions/SUBSCRIPTION_ID/resourceGroups/RES_GROUP_NAME/providers/Microsoft.ClassicStorage/storageAccounts/STORAGE_ACCOUNT_NAME/services/queue/queues/QUEUE_NAME"
 ```

Voor een (niet-klassiek) opslag account zou het metricTrigger het volgende omvatten:

```
"metricName": "ApproximateMessageCount",
"metricNamespace": "",
"metricResourceUri": "/subscriptions/SUBSCRIPTION_ID/resourceGroups/RES_GROUP_NAME/providers/Microsoft.Storage/storageAccounts/STORAGE_ACCOUNT_NAME/services/queue/queues/QUEUE_NAME"
```

## <a name="commonly-used-service-bus-metrics"></a>Veelgebruikte Service Bus metrische gegevens
U kunt schalen op Service Bus wachtrij lengte, het aantal berichten in de Service Bus wachtrij. Service Bus wachtrij lengte is een speciale metrische waarde en de drempel waarde is het aantal berichten per exemplaar. Als er bijvoorbeeld twee exemplaren zijn en als de drempel waarde is ingesteld op 100, wordt er geschaald wanneer het totale aantal berichten in de wachtrij 200 is. Dit kan 100 berichten per instantie, 120 en 80 of een andere combi natie van Maxi maal 200 of meer zijn.

Voor VM-schaal sets kunt u de instelling voor automatisch schalen in het Resource Manager-sjabloon bijwerken om *metrische waarde* als *ApproximateMessageCount* te gebruiken en de id van de opslag wachtrij als *metricResourceUri*door te geven.

```
"metricName": "MessageCount",
 "metricNamespace": "",
"metricResourceUri": "/subscriptions/SUBSCRIPTION_ID/resourceGroups/RES_GROUP_NAME/providers/Microsoft.ServiceBus/namespaces/SB_NAMESPACE/queues/QUEUE_NAME"
```

> [!NOTE]
> Voor Service Bus bestaat het concept van de resource groep niet, maar Azure Resource Manager maakt een standaard resource groep per regio. De resource groep bevindt zich doorgaans in de indeling ' default-ServiceBus-[regio] '. Bijvoorbeeld ' default-ServiceBus-oostelijke ', ' default-ServiceBus-Westus ', ' default-ServiceBus-AustraliaEast ' enzovoort.
>
>

