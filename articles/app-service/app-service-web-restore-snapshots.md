---
title: App herstellen vanuit een moment opname
description: Meer informatie over het herstellen van uw app vanuit een moment opname. Herstellen van onverwacht gegevens verlies in Premium-laag met de automatische schaduw kopieën.
author: ahmedelnably
ms.assetid: 4164f9b5-f735-41c6-a2bb-71f15cdda417
ms.topic: article
ms.date: 04/04/2018
ms.author: aelnably
ms.reviewer: nicking
ms.custom: seodec18
ms.openlocfilehash: e5b48e05d6bd9a310b4527300126464c4b755c8d
ms.sourcegitcommit: 265f1d6f3f4703daa8d0fc8a85cbd8acf0a17d30
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/02/2019
ms.locfileid: "74671143"
---
# <a name="restore-an-app-in-azure-from-a-snapshot"></a>Een app in azure herstellen vanuit een moment opname
In dit artikel wordt beschreven hoe u een app in [Azure app service](../app-service/overview.md) kunt herstellen vanuit een moment opname. U kunt uw app herstellen naar een eerdere status op basis van een van de moment opnamen van uw app. U hoeft geen back-up van moment opnamen in te scha kelen. het platform slaat automatisch een moment opname van alle apps op voor gegevens herstel doeleinden.

Moment opnamen zijn incrementele schaduw kopieën, en ze bieden diverse voor delen ten opzichte van regel matige [back-ups](manage-backup.md):
- Er zijn geen fouten bij het kopiëren van bestanden vanwege bestands vergrendelingen.
- Geen beperking van de opslag grootte.
- Er is geen configuratie vereist.

Herstellen vanaf moment opnamen is beschikbaar voor apps die worden uitgevoerd in de **Premium** -laag of hoger. Zie [een app omhoog schalen in azure](manage-scale-up.md)voor meer informatie over het omhoog schalen van uw app.

## <a name="limitations"></a>Beperkingen

- De functie is momenteel beschikbaar als preview-versie.
- U kunt alleen herstellen naar dezelfde app of naar een sleuf die bij die app hoort.
- App Service stopt de doel-app of doel sleuf tijdens het herstellen.
- App Service behoudt drie maanden aan moment opnamen voor herstel van platform gegevens.
- U kunt moment opnamen alleen herstellen voor de afgelopen 30 dagen.
- App Services die op een App Service Environment worden uitgevoerd, bieden geen ondersteuning voor moment opnamen.
 

## <a name="restore-an-app-from-a-snapshot"></a>Een app herstellen vanuit een moment opname

1. Klik op de pagina **instellingen** van uw app in de [Azure Portal](https://portal.azure.com)op **back-ups** om de pagina **back-ups** weer te geven. Klik vervolgens op **herstellen** onder het gedeelte **snap shot (preview)** .
   
    ![](./media/app-service-web-restore-snapshots/1.png)

2. Selecteer op de pagina **herstellen** de moment opname die u wilt herstellen.
   
    ![](./media/app-service-web-restore-snapshots/2.png)
   
3. Geef het doel op voor het herstellen van de app in de **terugzet bestemming**.
   
    ![](./media/app-service-web-restore-snapshots/3.png)
   
   > [!WARNING]
   > Als u **overschrijven**kiest, worden alle bestaande gegevens in het huidige bestands systeem van uw app gewist en overschreven. Voordat u op **OK**klikt, moet u ervoor zorgen dat het wat u wilt doen.
   > 
   > 
      
   > [!Note]
   > Als gevolg van actuele technische beperkingen kunt u alleen herstellen naar apps in dezelfde schaal eenheid. Deze beperking wordt in een toekomstige release verwijderd.
   > 
   > 
   
    U kunt een **bestaande app** selecteren om naar een sleuf te herstellen. Voordat u deze optie gebruikt, moet u al een sleuf hebben gemaakt in uw app.

4. U kunt ervoor kiezen om uw site configuratie te herstellen.
   
    ![](./media/app-service-web-restore-snapshots/4.png)

5. Klik op **OK**.
