---
title: HPC-cache van Azure beheren en bijwerken
description: De Azure HPC-cache beheren en bijwerken met behulp van de Azure Portal
author: ekpgh
ms.service: hpc-cache
ms.topic: conceptual
ms.date: 11/18/2019
ms.author: rohogue
ms.openlocfilehash: 9cd5ad151c977838fea30f52c7d4a93b4663c8ff
ms.sourcegitcommit: 4821b7b644d251593e211b150fcafa430c1accf0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/19/2019
ms.locfileid: "74166697"
---
# <a name="manage-your-cache-from-the-azure-portal"></a>Uw cache beheren via de Azure Portal

Op de pagina cache-overzicht in de Azure Portal worden project details, de cache status en de basis statistieken voor uw cache weer gegeven. Het bevat ook besturings elementen voor het verwijderen van de cache, het leegmaken van gegevens naar lange termijn opslag of het bijwerken van software.

Als u de overzichts pagina wilt openen, selecteert u uw cache resource in het Azure Portal. Laad bijvoorbeeld de pagina **alle resources** en klik op de naam van de cache.

![scherm afbeelding van de overzichts pagina van een Azure HPC-cache-exemplaar](media/hpc-cache-overview.png)

Met de knoppen boven aan de pagina kunt u de cache beheren:

* [**Flush**](#flush-cached-data) : Hiermee worden alle gegevens in de cache naar opslag doelen geschreven
* [**Upgrade**](#upgrade-cache-software) -de cache software bijwerken
* **Vernieuwen** : Hiermee wordt de pagina Overzicht opnieuw geladen
* [**Verwijderen**](#delete-the-cache) : de cache wordt definitief vernietigd

Meer informatie over deze opties vindt u hieronder.

## <a name="flush-cached-data"></a>In cache opgeslagen gegevens wissen

Met de knop **leegmaken** op de overzichts pagina wordt aan de cache meegedeeld dat alle gewijzigde gegevens die in de cache zijn opgeslagen, direct naar de back-end-opslag doelen worden geschreven. De cache slaat gegevens op in de opslag doelen, dus is het niet nodig om dit hand matig te doen, tenzij u ervoor wilt zorgen dat het back-end-opslag systeem up-to-date is. U kunt bijvoorbeeld **Flush** gebruiken voordat u een moment opname van de opslag maakt of de grootte van de gegevensset controleert.

> [!NOTE]
> Tijdens het leegmaak proces kan de cache geen client aanvragen verwerken. De toegang tot de cache wordt onderbroken en hervat nadat de bewerking is voltooid.

![scherm afbeelding van de bovenste knoppen met flush gemarkeerd en een pop-upbericht waarin de actie Flush wordt beschreven en u wordt gevraagd om door te gaan? met Ja (standaard) en geen knoppen](media/hpc-cache-flush.png)

Wanneer u de cache-leegmaak bewerking start, wordt het accepteren van client aanvragen door de cache gestopt en wordt de cache status op de overzichts pagina gewijzigd in **leegmaken**.

De gegevens in de cache worden opgeslagen in de juiste opslag doelen. Afhankelijk van de hoeveelheid gegevens die moet worden leeg gemaakt, kan het proces enkele minuten of meer tijd in beslag nemen.

Nadat alle gegevens zijn opgeslagen in opslag doelen, begint de cache automatisch met het uitvoeren van client aanvragen. De cache status wordt teruggezet naar **in orde**.

## <a name="upgrade-cache-software"></a>Cache software bijwerken

Als er een nieuwe software versie beschikbaar is, wordt de knop **bijwerken** actief. Boven aan de pagina wordt ook een bericht weer gegeven over het bijwerken van software.

![scherm afbeelding van de bovenste rij met knoppen waarop de knop bijwerken is ingeschakeld](media/hpc-cache-upgrade-button.png)

Client toegang wordt niet onderbroken tijdens een software-upgrade, maar de prestaties van de cache worden trager. Upgrade van software plannen tijdens niet-piek uren of in een geplande onderhouds periode.

Het kan enkele uren duren voordat de software-update is uitgevoerd. Caches die zijn geconfigureerd met een hogere door Voer, nemen meer tijd in beslag dan caches met kleinere waarden voor piek doorvoer.

Wanneer een software-upgrade beschikbaar is, hebt u een week of zodat deze hand matig kan worden toegepast. De eind datum wordt vermeld in het upgrade bericht. Als u tijdens die tijd niet bijwerkt, past Azure automatisch de update toe op uw cache. De timing van de automatische upgrade kan niet worden geconfigureerd. Als u zich zorgen maakt over de invloed op de cache prestaties, moet u de software zelf upgraden voordat de periode verloopt.

Klik op de knop **bijwerken** om de software-update te starten. De cache status verandert in **een upgrade** totdat de bewerking is voltooid.

## <a name="delete-the-cache"></a>De cache verwijderen

De knop **verwijderen** vernietigt de cache. Wanneer u een cache verwijdert, worden alle bijbehorende bronnen vernietigd en worden er geen account kosten meer in rekening gebracht.

Opslag doelen worden niet beïnvloed wanneer u de cache verwijdert. U kunt ze later toevoegen aan een toekomstige cache of ze afzonderlijk uit bedrijf nemen.

In de cache worden niet-opgeslagen gegevens automatisch naar opslag doelen verwijderd als onderdeel van de laatste afsluiting.

## <a name="cache-metrics-and-monitoring"></a>Metrische gegevens over cache en bewaking

Op de overzichts pagina worden grafieken weer gegeven voor een aantal basis cache-statistieken, bewerkingen per seconde en latentie.

![scherm afbeelding van drie lijn grafieken met de statistieken die hierboven worden vermeld voor een voorbeeld cache](media/hpc-cache-overview-stats.png)

Deze grafieken maken deel uit van de ingebouwde hulpprogram ma's voor bewaking en analyse van Azure. Aanvullende hulp middelen en waarschuwingen zijn beschikbaar op de pagina's onder de kop **bewaking** in de zijbalk van de portal. Meer informatie vindt u in de sectie Portal van de [Azure monitoring-documentatie](../azure-monitor/insights/monitor-azure-resource.md#monitoring-in-the-azure-portal).

## <a name="next-steps"></a>Volgende stappen

<!-- * Learn more about metrics and statistics for hpc cache -->
* Meer informatie over de [hulpprogram ma's voor metrische gegevens en statistieken van Azure](../azure-monitor/index.yml)
* [Hulp krijgen bij uw Azure HPC-cache](hpc-cache-support-ticket.md)
