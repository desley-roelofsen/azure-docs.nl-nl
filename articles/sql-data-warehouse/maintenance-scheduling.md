---
title: Onderhouds planningen voor Azure
description: Met onderhouds planning kunnen klanten de nodige geplande onderhouds gebeurtenissen plannen die de Azure SQL Data Warehouse-service gebruikt om nieuwe functies, upgrades en patches uit te rollen.
services: sql-data-warehouse
author: antvgski
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: design
ms.date: 11/07/2019
ms.author: anvang
ms.reviewer: jrasnick
ms.openlocfilehash: e9d5a137247c072516c0b25d7f6147ef48fec248
ms.sourcegitcommit: 35715a7df8e476286e3fee954818ae1278cef1fc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/08/2019
ms.locfileid: "73839795"
---
# <a name="use-maintenance-schedules-to-manage-service-updates-and-maintenance"></a>Onderhouds planningen gebruiken voor het beheren van service-updates en-onderhoud

De functie onderhouds planning integreert de Service Health geplande onderhouds meldingen, Resource Health controle en de Azure SQL Data Warehouse onderhouds plannings service.

U moet de onderhouds planning gebruiken om een tijd venster te kiezen wanneer het handig is om nieuwe functies, upgrades en patches te ontvangen. U moet een primair en secundair onderhouds venster binnen een periode van zeven dagen kiezen. elk venster moet zich binnen een bereik van de dag bevallen.

U kunt bijvoorbeeld een primair venster van zaterdag 22:00 op zondag 01:00 plannen en vervolgens een secundair venster van woensdag 19:00 tot 22:00 plannen. Als SQL Data Warehouse geen onderhoud kunt uitvoeren tijdens het primaire onderhouds venster, wordt het onderhoud opnieuw geprobeerd tijdens het secundaire onderhouds venster. Service onderhoud kan tijdens het primaire en het secundaire Windows optreden. Om ervoor te zorgen dat alle onderhouds bewerkingen snel worden voltooid, kunnen DW400c en lagere Data Warehouse-lagen onderhoud volt ooien buiten een aangewezen onderhouds venster.

Voor alle nieuw gemaakte Azure SQL Data Warehouse instanties wordt een door het systeem gedefinieerde onderhouds planning toegepast tijdens de implementatie. De planning kan worden bewerkt zodra de implementatie is voltooid.

Hoewel een onderhouds venster tussen drie en acht uur kan zijn, betekent dit niet dat het Data Warehouse offline is voor de duur. Onderhoud kan op elk gewenst moment in dat venster optreden en u verwacht dat er een enkele verbreking tijdens deze periode verloopt ~ 5 -6 minuten, aangezien de service nieuwe code implementeert in uw data warehouse. DW400c en lager kunnen meerdere korte verliezen veroorzaken in connectiviteit op verschillende momenten tijdens het onderhouds venster. Als onderhoud wordt gestart, worden alle actieve sessies geannuleerd en worden niet-doorgevoerde trans acties teruggedraaid. Zorg ervoor dat uw data warehouse geen langlopende trans acties heeft vóór de gekozen onderhouds periode om de uitval tijd van het exemplaar te minimaliseren.

Alle onderhouds bewerkingen moeten binnen de opgegeven onderhouds Vensters worden voltooid, tenzij we een tijd gevoelige update moeten implementeren. Als uw data warehouse tijdens een gepland onderhoud wordt onderbroken, wordt het bijgewerkt tijdens de hervatting. U ontvangt een melding zodra uw data warehouse-onderhoud is voltooid.

## <a name="alerts-and-monitoring"></a>Waarschuwingen en bewaking

Dankzij de integratie met Service Health meldingen en de Resource Health controle controle kunnen klanten op de hoogte blijven van de bedreigende onderhouds activiteiten. Deze automatisering maakt gebruik van Azure Monitor. U kunt bepalen hoe u op de hoogte wilt worden gesteld van aanstaande onderhouds gebeurtenissen. Daarnaast kunt u kiezen welke automatische stromen u helpen uitval tijd te beheren en de operationele impact te minimaliseren.
Er wordt een melding van 24 uur voorgeschoten vóór alle onderhouds gebeurtenissen die niet voor de DWC400c en lagere lagen zijn.

> [!NOTE]
> In het geval dat er een essentiële update moet worden geïmplementeerd, kunnen geavanceerde meldings tijden aanzienlijk worden verkleind.

Als u een voorschot bericht hebt ontvangen dat er onderhoud wordt uitgevoerd, maar SQL Data Warehouse geen onderhoud kan uitvoeren tijdens die periode, ontvangt u een melding over de annulering. Het onderhoud wordt vervolgens hervat tijdens de volgende geplande onderhouds periode.

Alle actieve onderhouds gebeurtenissen worden weer gegeven in de sectie **service Health gepland onderhoud** . De Service Health geschiedenis bevat een volledige record met gebeurtenissen in het verleden. U kunt onderhoud bewaken via het Azure Service Health dash board van de portal controleren tijdens een actieve gebeurtenis.

### <a name="maintenance-schedule-availability"></a>Beschik baarheid onderhouds planning

Zelfs als de onderhouds planning niet beschikbaar is in de geselecteerde regio, kunt u op elk gewenst moment uw onderhouds planning weer geven en bewerken. Wanneer de onderhouds planning in uw regio beschikbaar wordt, wordt de geïdentificeerde planning onmiddellijk actief in uw data warehouse.

## <a name="view-a-maintenance-schedule"></a>Een onderhouds planning weer geven 

### <a name="portal"></a>Portal

De standaardinstelling is dat voor alle nieuwe Azure SQL Data Warehouse-exemplaren een primaire en secundaire onderhoudsperiode van acht uur wordt toegepast tijdens de implementatie. Zoals hierboven aangegeven, kunt u de Windows-installatie wijzigen zodra de implementatie is voltooid. Er vindt geen onderhoud plaats buiten de opgegeven onderhoudsperioden zonder voorafgaande kennisgeving.

Als u de onderhoudsplanning wilt weergeven die op uw datawarehouse is toegepast, voert u de volgende stappen uit:

1.  Meld u aan bij de [Azure Portal](https://portal.azure.com/).
2.  Selecteer het data warehouse dat u wilt weer geven. 
3.  Het geselecteerde data warehouse wordt geopend op de Blade overzicht. De onderhouds planning die wordt toegepast op het Data Warehouse wordt onder **onderhouds planning**weer gegeven.

![Blade overzicht](media/sql-data-warehouse-maintenance-scheduling/clear-overview-blade.PNG)

## <a name="change-a-maintenance-schedule"></a>Een onderhouds planning wijzigen 

### <a name="portal"></a>Portal
Een onderhouds planning kan op elk gewenst moment worden bijgewerkt of gewijzigd. Als de geselecteerde instantie een actieve onderhouds cyclus doorloopt, worden de instellingen opgeslagen. Ze worden tijdens de volgende geïdentificeerde onderhouds periode actief. Meer [informatie](https://docs.microsoft.com/azure/service-health/resource-health-overview) over het bewaken van uw data warehouse tijdens een actieve onderhouds gebeurtenis. 

### <a name="identifying-the-primary-and-secondary-windows"></a>De primaire en secundaire Windows identificeren

Het primaire en het secundaire Windows-venster moeten verschillende datumbereiken hebben. Een voor beeld is een primair venster van dinsdag – donderdag en een secundaire periode van zaterdag – zondag.

Als u de onderhouds planning voor uw data warehouse wilt wijzigen, voert u de volgende stappen uit:
1.  Meld u aan bij de [Azure Portal](https://portal.azure.com/).
2.  Selecteer het data warehouse dat u wilt bijwerken. De pagina wordt geopend op de Blade overzicht. 
3.  Open de pagina voor onderhouds plannings instellingen door de **overzichts koppeling onderhouds planning (preview)** op de Blade overzicht te selecteren. U kunt ook de optie **onderhouds planning** selecteren in het menu resource aan de linkerkant.  

    ![Blade opties voor overzicht](media/sql-data-warehouse-maintenance-scheduling/maintenance-change-option.png)

4. Bepaal het bereik van de voor keur dag voor uw primaire onderhouds venster met behulp van de opties boven aan de pagina. Deze selectie bepaalt of uw primaire venster wordt uitgevoerd op een weekdag of in het weekend. Met uw selectie worden de vervolg keuzelijst waarden bijgewerkt. Tijdens de preview-periode is het mogelijk dat bepaalde regio's nog geen ondersteuning bieden voor de volledige set opties voor beschik bare **dagen** .

   ![Blade onderhouds instellingen](media/sql-data-warehouse-maintenance-scheduling/maintenance-settings-page.png)

5. Kies uw favoriete primaire en secundaire onderhouds Vensters met behulp van de vervolg keuzelijst:
   - **Dag**: de voorkeurs dag voor het uitvoeren van onderhoud tijdens het geselecteerde venster.
   - **Begin tijd**: de voorkeurs start tijd voor het onderhouds venster.
   - **Tijd venster**: voorkeurs duur van uw tijd venster.

   Het gebied **samen vatting van planning** onder aan de Blade wordt bijgewerkt op basis van de waarden die u hebt geselecteerd. 
  
6. Selecteer **Opslaan**. Er wordt een bericht weer gegeven waarin wordt bevestigd dat uw nieuwe planning nu actief is. 

   Als u een planning opslaat in een regio die geen onderhouds planning ondersteunt, wordt het volgende bericht weer gegeven. Uw instellingen worden opgeslagen en worden actief wanneer de functie beschikbaar wordt in de geselecteerde regio.    

   ![Bericht over beschik baarheid van regio's](media/sql-data-warehouse-maintenance-scheduling/maintenance-notactive-toast.png)

## <a name="next-steps"></a>Volgende stappen
- Meer [informatie](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-alerts-unified-usage) over het maken, weer geven en beheren van waarschuwingen met behulp van Azure monitor.
- Meer [informatie](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-alerts-unified-log-webhook) over webhook-acties voor logboek waarschuwings regels.
- [Meer informatie](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-action-groups) Actie groepen maken en beheren.
- Meer [informatie](https://docs.microsoft.com/azure/service-health/service-health-overview) over Azure service health.
