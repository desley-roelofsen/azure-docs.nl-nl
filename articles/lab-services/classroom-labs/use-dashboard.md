---
title: Dash board gebruiken voor een leslokaal Lab in Azure Lab Services | Microsoft Docs
description: Meer informatie over het gebruik van Dash boards voor een leslokaal Lab in Azure Lab Services.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/31/2019
ms.author: spelluru
ms.openlocfilehash: d1e34a493b747383ce479bcad638098b41e59d2b
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/04/2019
ms.locfileid: "73588185"
---
# <a name="dashboard-for-classroom-labs"></a>Dash board voor klassikale Labs
In dit artikel wordt de dashboard weergave van een leslokaal Lab in Azure Lab Services beschreven. 

![Dashboard](../media/use-dashboard/dashboard.png)

## <a name="costs-and-billing-tile"></a>Tegel kosten en facturering
Deze tegel biedt de volgende details van de kosten raming:

| Instelling | Waarde | 
| ------- | ----- | 
| Quotum uren | Het maximale aantal uren dat een gebruiker de virtuele machine buiten de geplande uren mag gebruiken. |
| Geplande uren | Uren die worden gemaakt op basis van de planning die in het lab is ingesteld. Deze waarde is alleen beschikbaar als er een from/to-datum is ingesteld voor alle Schedule-gebeurtenissen. |
| Uur/gebruiker | De som van de quotum uren en de geplande uren. |
| Maximum aantal gebruikers | Het maximum aantal gebruikers in het lab op basis van alle virtuele machines die moeten worden geclaimd. |
| Uur x gebruikers | Uren/gebruiker vermenigvuldigd met het aantal gebruikers. |
| Aangepast quotum | De som van de quotum uren die zijn toegevoegd aan specifieke gebruikers. |
| Totaal aantal uren * $/uur | De kosten per uur op basis van de geselecteerde VM-grootte. Dit is gebaseerd op de prijs regel matig betalen naar gebruik. |
| Totale geschatte kosten | Dit is de maximale prijs voor dit lab op basis van de huidige instellingen. |

## <a name="template-tile"></a>Sjabloon tegel
U ziet de volgende informatie op deze tegel:

- De datum waarop de sjabloon is gemaakt 
- De datum waarop de sjabloon voor het laatst is gepubliceerd 

Het bevat ook een koppeling om naar de **sjabloon** pagina te gaan waar u [de sjabloon-VM](how-to-create-manage-template.md) voor de klasse kunt beheren. 

## <a name="virtual-machine-pool-tile"></a>Tegel virtuele-machine groep

U ziet de volgende informatie op deze tegel:

- Aantal virtuele machines dat is toegewezen aan studenten (gebruikers)
- Aantal virtuele machines die nog niet zijn toegewezen aan studenten

Het bevat ook een koppeling naar de pagina met de **virtuele-machine groep** waar u [de pool van virtuele machines](how-to-set-virtual-machine-passwords.md) in de test omgeving kunt beheren. 

## <a name="users-tile"></a>Tegel gebruikers

U ziet de volgende informatie op deze tegel:

- Aantal gebruikers dat is geregistreerd voor de klasse
- Het aantal gebruikers dat is toegevoegd aan het lab, maar niet is geregistreerd voor de klasse 

Het bevat ook een koppeling om naar de pagina **gebruikers** te gaan waar u [gebruikers](how-to-configure-student-usage.md) voor het lab kunt beheren. 

## <a name="schedules-tile"></a>Tegel schema's
U ziet de huidige geplande gebeurtenissen voor het lab op de tegel. Het bevat ook een koppeling om naar de **plannings** pagina te gaan waar u [schema's kunt maken en beheren](how-to-create-schedules.md). De tegel toont u Details voor slechts twee geplande gebeurtenissen en het aantal resterende geplande gebeurtenissen voor het lab. 

![Geplande gebeurtenissen](../media/use-dashboard/scheduled-events.png)

