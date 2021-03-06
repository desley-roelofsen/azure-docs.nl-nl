---
title: Continue bewaking van uw DevOps release-pijp lijn met Azure-pijp lijnen en Azure-toepassing inzichten | Microsoft Docs
description: Biedt instructies voor het snel instellen van doorlopende bewaking met Application Insights
ms.service: azure-monitor
ms.subservice: application-insights
ms.topic: conceptual
author: mrbullwinkle
ms.author: mbullwin
ms.date: 07/16/2019
ms.openlocfilehash: c891cc7564a60dfd665d84d569e266332d255fa9
ms.sourcegitcommit: 1bd2207c69a0c45076848a094292735faa012d22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/21/2019
ms.locfileid: "72677521"
---
# <a name="add-continuous-monitoring-to-your-release-pipeline"></a>Doorlopende bewaking aan uw release pijplijn toevoegen

Azure-pijp lijnen worden geïntegreerd met Azure-toepassing Insights, zodat uw DevOps-release pijplijn gedurende de levens cyclus van software ontwikkeling voortdurend kan worden bewaakt. 

Met continue bewaking kunnen release pijplijnen bewakings gegevens omvatten van Application Insights en andere Azure-resources. Wanneer de release-pijp lijn een Application Insights waarschuwing detecteert, kan de pijp lijn de implementatie destappen of terugdraaien totdat de waarschuwing is opgelost. Als alle controles zijn geslaagd, kunnen implementaties automatisch door gaan, van het testen van de gehele productie, zonder hand matige tussen komst. 

## <a name="configure-continuous-monitoring"></a>Continue bewaking configureren

1. Selecteer een organisatie en project in [Azure DevOps](https://dev.azure.com).
   
1. Selecteer in het linkermenu op de pagina project **pijp lijnen**  > **releases**. 
   
1. Vervolg de pijl naast **Nieuw** en selecteer **nieuwe release pijplijn**. Als u nog geen pijp lijn hebt, selecteert u **nieuwe pijp lijn** op de pagina die wordt weer gegeven.
   
1. Zoek en selecteer in het deel venster **sjabloon selecteren** de optie **Azure app service implementatie met doorlopende bewaking**en selecteer vervolgens **Toep assen**. 

   ![Nieuwe release pijplijn voor Azure-pijp lijnen](media/continuous-monitoring/001.png)

1. Selecteer in het vak **fase 1** de Hyper link om **fase taken weer te geven.**

   ![Fase taken weer geven](media/continuous-monitoring/002.png)

1. In het deel venster **fase 1** -configuratie vult u de volgende velden in: 

    | Parameter        | Waarde |
   | ------------- |:-----|
   | **Fase naam**      | Geef een naam op voor de fase of geef deze op in **fase 1**. |
   | **Azure-abonnement** | Vervolg keuzelijst en selecteer het gekoppelde Azure-abonnement dat u wilt gebruiken.|
   | **App-type** | Vervolg keuzelijst en selecteer het type app. |
   | **App Service naam** | Voer de naam van uw Azure App Service in. |
   | **Naam van de resource groep voor Application Insights**    | Vervolg keuzelijst en selecteer de resource groep die u wilt gebruiken. |
   | **Resource naam Application Insights** | Vervolg keuzelijst en selecteer de Application Insights resource voor de resource groep die u hebt geselecteerd.

1. Als u de pijp lijn met de standaard instellingen voor waarschuwings regels wilt opslaan, selecteert u in het Azure DevOps-venster rechtsboven **Opslaan** . Voer een beschrijvende opmerking in en selecteer **OK**.

## <a name="modify-alert-rules"></a>Waarschuwings regels wijzigen

Out of box heeft de **Azure app service-implementatie met continue monitoring** sjabloon vier waarschuwings regels: **Beschik baarheid**, **mislukte aanvragen**, **reactie tijd van server**en **Server uitzonderingen**. U kunt meer regels toevoegen of de regel instellingen wijzigen om te voldoen aan de vereisten van uw service niveau. 

Instellingen voor waarschuwings regels wijzigen:

1. Selecteer in het linkerdeel venster van de pagina release-pijp lijn de optie **Application Insights waarschuwingen configureren**.

1. Selecteer in het deel venster **waarschuwingen Azure monitor** het beletsel teken **...** naast **waarschuwings regels**.
   
1. Selecteer in het dialoog venster **waarschuwings regels** de vervolg keuzelijst naast een waarschuwings regel, zoals **Beschik baarheid**. 
   
1. Wijzig de **drempel waarden** en andere instellingen om te voldoen aan uw vereisten.
   
   ![Waarschuwing wijzigen](media/continuous-monitoring/003.png)
   
1. Selecteer **OK**en selecteer vervolgens **Opslaan** in de rechter bovenhoek in het Azure DevOps-venster. Voer een beschrijvende opmerking in en selecteer **OK**.

## <a name="add-deployment-conditions"></a>Implementatie voorwaarden toevoegen

Wanneer u implementatie-Gates toevoegt aan uw release pijplijn, voor komt een waarschuwing die de drempel waarden overschrijdt die u instelt, voor komt een ongewenste release promotie. Zodra u de waarschuwing hebt opgelost, kan de implementatie automatisch worden voortgezet.

Implementatie-Gates toevoegen:

1. Selecteer op de hoofd pagina van de pijp lijn onder **stadia**het symbool **voor waarden vóór de implementatie** of na de **implementatie** , afhankelijk van welke fase een continue bewakings poort nodig heeft.
   
   ![Voor waarden voorafgaand aan implementatie](media/continuous-monitoring/004.png)
   
1. Stel in het configuratie venster **voor waarden voor voor implementatie** de **poorten** in op **ingeschakeld**.
   
1. Selecteer **toevoegen**naast **implementatie poorten**.
   
1. Selecteer **Query Azure monitor waarschuwingen** in het vervolg keuzemenu. Met deze optie krijgt u toegang tot zowel Azure Monitor als Application Insights-waarschuwingen.
   
   ![Azure Monitor waarschuwingen voor query's](media/continuous-monitoring/005.png)
   
1. Onder **evaluatie opties**voert u de gewenste waarden in voor instellingen, zoals **de tijd tussen het opnieuw evalueren van poorten** en **de time-out waarna de poorten mislukken**. 

## <a name="view-release-logs"></a>Release logboeken weer geven

U kunt het gedrag van de implementatie poort en andere release stappen in de release logboeken bekijken. De logboeken openen:

1. Selecteer **releases** in het menu links van de pijplijn pagina. 
   
1. Selecteer een release. 
   
1. Selecteer onder **fases**een wille keurige fase om een release overzicht weer te geven. 
   
1. Als u logboeken wilt weer geven, selecteert u **Logboeken weer geven** in het overzicht van de release, selecteert u de Hyper link **geslaagd** of **mislukt** in een wille keurig stadium, of **beweegt u de**muis aanwijzer over een wille keurig 
   
   ![Release logboeken weer geven](media/continuous-monitoring/006.png)

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de [documentatie over Azure-pijp lijnen](https://docs.microsoft.com/azure/devops/pipelines)voor meer informatie over Azure-pijp lijnen.
