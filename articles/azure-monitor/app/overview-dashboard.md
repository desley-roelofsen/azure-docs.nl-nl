---
title: Overzichts dashboard voor Azure-toepassing Insights | Microsoft Docs
description: Bewaak toepassingen met Azure-toepassing inzicht en overzicht dashboard functionaliteit.
ms.service: azure-monitor
ms.subservice: application-insights
ms.topic: conceptual
author: mrbullwinkle
ms.author: mbullwin
ms.date: 06/03/2019
ms.openlocfilehash: 45c04cc32733873061bd6ff44b2e5484b106a893
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/25/2019
ms.locfileid: "75406644"
---
# <a name="application-insights-overview-dashboard"></a>Application Insights-overzichts dashboard

Application Insights heeft altijd een overzichts venster weer gegeven waarmee u de status en prestaties van uw toepassing snel, in één oogopslag kunt beoordelen. Het nieuwe overzichts dashboard biedt een snellere, flexibeler ervaring.

## <a name="how-do-i-test-out-the-new-experience"></a>Hoe kan ik de nieuwe ervaring testen?

Het nieuwe overzichts dashboard wordt nu standaard gestart:

![Voorbeeld venster Overzicht](./media/overview-dashboard/overview.png)

## <a name="better-performance"></a>Betere prestaties

De selectie van het tijds bereik is vereenvoudigd tot een eenvoudige interface met één klik.

![Tijdsbereik](./media/overview-dashboard/app-insights-overview-dashboard-03.png)

De algehele prestaties zijn aanzienlijk verhoogd. U hebt één klik toegang tot populaire functies, zoals **zoeken** en **analyses**. Elke standaard KPI-tegel die dynamisch wordt bijgewerkt, biedt inzicht in de bijbehorende Application Insights-functies. Voor meer informatie over mislukte aanvragen selecteert u **fouten** in de kop van de **onderzoek** :

![Fouten](./media/overview-dashboard/app-insights-overview-dashboard-04.png)

## <a name="application-dashboard"></a>Toepassingsdashboard

Toepassings dashboard maakt gebruik van de bestaande dash board-technologie in azure om een volledig aanpas bare weer gave van uw toepassings status en-prestaties te bieden.

Als u het standaard dashboard wilt openen, selecteert u _toepassings dashboard_ in de linkerbovenhoek.

![Dashboardweergave](./media/overview-dashboard/app-insights-overview-dashboard-05.png)

Als dit de eerste keer is dat u het dash board opent, wordt er een standaard weergave geopend:

![Dashboardweergave](./media/overview-dashboard/0001-dashboard.png)

U kunt de standaard weergave gebruiken als u dat wilt. U kunt ook toevoegen en verwijderen uit het dash board om het beste te voldoen aan de behoeften van uw team.

> [!NOTE]
> Alle gebruikers met toegang tot de Application Insights resource delen dezelfde toepassings dashboard ervaring. Wijzigingen die door een gebruiker zijn aangebracht, wijzigen de weer gave voor alle gebruikers.

Als u terug wilt gaan naar de overzichts ervaring, selecteert u:

![Knop overzicht](./media/overview-dashboard/app-insights-overview-dashboard-07.png)

## <a name="troubleshooting"></a>Problemen oplossen

Als u **tegel instellingen configureren** selecteert en een aangepast tijds bereik van meer dan 31 dagen instelt, wordt uw dash board niet meer dan 31 dagen aan gegevens weer gegeven, zelfs met de standaard Bewaar periode van 90 dagen. Er is momenteel geen oplossing voor dit gedrag.

## <a name="next-steps"></a>Volgende stappen

- [Trechters](../../azure-monitor/app/usage-funnels.md)
- [Retentie](../../azure-monitor/app/usage-retention.md)
- [Gebruikersstromen](../../azure-monitor/app/usage-flows.md)
