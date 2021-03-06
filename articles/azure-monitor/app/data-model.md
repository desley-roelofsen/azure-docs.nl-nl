---
title: Telemetrie-gegevens model van Azure-toepassing Insights | Microsoft Docs
description: Overzicht van Application Insights-gegevens model
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.service: azure-monitor
ms.subservice: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 10/14/2019
ms.reviewer: sergkanz
ms.author: mbullwin
ms.openlocfilehash: b14ce5cc83dcbbeef2379d21027d4bca337fd9fd
ms.sourcegitcommit: 0576bcb894031eb9e7ddb919e241e2e3c42f291d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/15/2019
ms.locfileid: "72376026"
---
# <a name="application-insights-telemetry-data-model"></a>Application Insights telemetrie-gegevens model

[Azure-toepassing Insights](../../azure-monitor/app/app-insights-overview.md) verzendt telemetrie van uw webtoepassing naar de Azure Portal, zodat u de prestaties en het gebruik van uw toepassing kunt analyseren. Het telemetrie-model is standaard, zodat het mogelijk is om platform-en taal onafhankelijke bewaking te maken. 

Gegevens die worden verzameld door Application Insights modellen dit typische toepassings uitvoerings patroon:

![Application Insights toepassings model](./media/data-model/application-insights-data-model.png)

De volgende typen telemetrie worden gebruikt voor het bewaken van de uitvoering van uw app. De volgende drie typen worden doorgaans automatisch verzameld door de Application Insights SDK van het Framework voor webtoepassingen:

* [**Aanvraag**](data-model-request-telemetry.md) -gegenereerd om een aanvraag te registreren die door uw app is ontvangen. Zo genereert de Application Insights Web SDK automatisch een telemetrie-aanvraag voor elke HTTP-aanvraag die uw web-app ontvangt. 

    Een **bewerking** is de threads van de uitvoering waarmee een aanvraag wordt verwerkt. U kunt ook [code schrijven](../../azure-monitor/app/api-custom-events-metrics.md#trackrequest) voor het bewaken van andere soorten bewerkingen, zoals een ' Wake up ' in een Webtaak of functie waarmee periodiek gegevens worden verwerkt.  Elke bewerking heeft een ID. Deze ID die kan worden gebruikt voor het [groeperen](../../azure-monitor/app/correlation.md) van alle telemetrie die zijn gegenereerd tijdens de verwerking van de aanvraag door uw app. Elke bewerking slaagt of mislukt en heeft een tijds duur.
* [**Uitzonde ring**](data-model-exception-telemetry.md) -geeft meestal een uitzonde ring aan die ervoor zorgt dat een bewerking mislukt.
* [**Afhankelijkheid**](data-model-dependency-telemetry.md) : vertegenwoordigt een aanroep van uw app naar een externe service of opslag, zoals een rest API of SQL. In ASP.NET worden afhankelijkheids aanroepen naar SQL gedefinieerd door `System.Data`. Aanroepen van HTTP-eind punten worden gedefinieerd door `System.Net`. 

Application Insights biedt drie extra gegevens typen voor aangepaste telemetrie:

* [Trace](data-model-trace-telemetry.md) : wordt direct gebruikt, of via een adapter voor het implementeren van diagnostische logboek registratie met behulp van een instrumentatie raamwerk dat bekend is bij u, zoals `Log4Net` of `System.Diagnostics`.
* [Gebeurtenis](data-model-event-telemetry.md) : wordt meestal gebruikt voor het vastleggen van gebruikers interactie met uw service, voor het analyseren van gebruiks patronen.
* [Metrisch](data-model-metric-telemetry.md) : wordt gebruikt voor het rapporteren van periodieke scalaire metingen.

Elk telemetrie-item kan de [context informatie](data-model-context.md) definiëren, zoals de toepassings versie of de gebruikers sessie-id. Context is een set sterk getypeerde velden waarmee bepaalde scenario's worden gedeblokkeerd. Wanneer de versie van de toepassing correct is geïnitialiseerd, kunnen Application Insights nieuwe patronen detecteren in het toepassings gedrag, gecorreleerd met een nieuwe implementatie. Sessie-id kan worden gebruikt voor het berekenen van de storing of het effect van een probleem op gebruikers. Het berekenen van het unieke aantal sessie-id-waarden voor een bepaalde mislukte afhankelijkheid, fout tracering of kritieke uitzonde ring geeft een goed beeld van een impact.

Application Insights telemetrie-model definieert een manier om telemetrie te [correleren](../../azure-monitor/app/correlation.md) aan de bewerking waarvan het deel uitmaakt. Een aanvraag kan bijvoorbeeld een SQL Database aanroepen en vastgelegde diagnostische gegevens worden gemaakt. U kunt de correlatie context instellen voor de telemetrie-items die deze weer koppelen aan de telemetrie van de aanvraag.

## <a name="schema-improvements"></a>Verbeteringen in het schema

Application Insights gegevens model is een eenvoudige en eenvoudig te bieden, maar krachtige manier om de telemetrie van uw toepassing te model leren. We streven ernaar het model eenvoudig en dun te laten werken om essentiële scenario's te ondersteunen en het schema voor Geavanceerd gebruik toe te staan.

Voor het rapporteren van gegevens model-of schema problemen en suggesties gebruikt u GitHub [ApplicationInsights-basismap](https://github.com/Microsoft/ApplicationInsights-Home/issues) .

## <a name="next-steps"></a>Volgende stappen

- [Aangepaste telemetrie schrijven](../../azure-monitor/app/api-custom-events-metrics.md)
- Meer informatie over hoe u [telemetrie kunt uitbreiden en filteren](../../azure-monitor/app/api-filtering-sampling.md).
- Gebruik [steek proeven](../../azure-monitor/app/sampling.md) om de hoeveelheid telemetrie op basis van het gegevens model te minimaliseren.
- Bekijk de [platforms](../../azure-monitor/app/platforms.md) die door Application Insights worden ondersteund.
