---
author: rothja
ms.service: cost-management-billing
ms.topic: include
ms.date: 08/09/2019
ms.author: jroth
ms.openlocfilehash: a05229f22e1682d73b627e3b4644ba64e653335f
ms.sourcegitcommit: dd0304e3a17ab36e02cf9148d5fe22deaac18118
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/22/2019
ms.locfileid: "74414551"
---
| Bron | Gratis | Gedeeld | Basic | Standard | Premium (v2) | Isolated </th> |
| --- | --- | --- | --- | --- | --- | --- |
| [Web-, mobiele of API-apps](https://azure.microsoft.com/services/app-service/) per [Azure app service abonnement](../articles/app-service/overview-hosting-plans.md)<sup>1</sup> |10 |100 |Onbeperkt<sup>2</sup> |Onbeperkt<sup>2</sup> |Onbeperkt<sup>2</sup> |Onbeperkt<sup>2</sup>|
| [App Service-plan](../articles/app-service/overview-hosting-plans.md) |10 per regio |10 per resource groep |100 per resource groep |100 per resource groep |100 per resource groep |100 per resource groep|
| Type Compute-instantie |Gedeeld |Gedeeld |Toegewezen<sup>3</sup> |Toegewezen<sup>3</sup> |Toegewezen<sup>3</sup></p> |Toegewezen<sup>3</sup>|
| [Uitschalen](../articles/app-service/manage-scale-up.md) (maximum aantal instanties) |1 gedeeld |1 gedeeld |3 toegewezen<sup>3</sup> |10 toegewezen<sup>3</sup> |30 toegewezen<sup>3</sup>|100 toegewezen<sup>4</sup>|
| Opslag<sup>5</sup> |1 GB<sup>5</sup> |1 GB<sup>5</sup> |10 GB<sup>5</sup> |50 GB<sup>5</sup> |250 GB<sup>5</sup></p> |1 TB<sup>5</sup>|
| CPU-tijd (5 minuten)<sup>6</sup> |3 minuten |3 minuten |Onbeperkt, betalen tegen standaard [tarieven](https://azure.microsoft.com/pricing/details/app-service/)</a> |Onbeperkt, betalen tegen standaard [tarieven](https://azure.microsoft.com/pricing/details/app-service/)</a> |Onbeperkt, betalen tegen standaard [tarieven](https://azure.microsoft.com/pricing/details/app-service/)</a> |Onbeperkt, betalen tegen standaard [tarieven](https://azure.microsoft.com/pricing/details/app-service/)</a>|
| CPU-tijd (dag)<sup>6</sup> |60 minuten |240 minuten |Onbeperkt, betalen tegen standaard [tarieven](https://azure.microsoft.com/pricing/details/app-service/)</a> |Onbeperkt, betalen tegen standaard [tarieven](https://azure.microsoft.com/pricing/details/app-service/)</a> |Onbeperkt, betalen tegen standaard [tarieven](https://azure.microsoft.com/pricing/details/app-service/)</a> |Onbeperkt, betalen tegen standaard [tarieven](https://azure.microsoft.com/pricing/details/app-service/)</a> |
| Geheugen (1 uur) |1\.024 MB per App Service-abonnement |1\.024 MB per app |N/A |N/A |N/A |N/A |
| Bandbreedte |165 MB |Onbeperkte, [tarieven voor gegevens overdracht](https://azure.microsoft.com/pricing/details/data-transfers/) zijn van toepassing |Onbeperkte, [tarieven voor gegevens overdracht](https://azure.microsoft.com/pricing/details/data-transfers/) zijn van toepassing |Onbeperkte, [tarieven voor gegevens overdracht](https://azure.microsoft.com/pricing/details/data-transfers/) zijn van toepassing |Onbeperkte, [tarieven voor gegevens overdracht](https://azure.microsoft.com/pricing/details/data-transfers/) zijn van toepassing |Onbeperkte, [tarieven voor gegevens overdracht](https://azure.microsoft.com/pricing/details/data-transfers/) zijn van toepassing |
| Toepassingsarchitectuur |32-bits |32-bits |32-bits/64-bits |32-bits/64-bits |32-bits/64-bits |32-bits/64-bits |
| Websockets per exemplaar<sup>7</sup> |5 |35 |350 |Onbeperkt |Onbeperkt |Onbeperkt |
| IP-verbindingen | 600 | 600 | Is afhankelijk van de grootte van het exemplaar<sup>8</sup> | Is afhankelijk van de grootte van het exemplaar<sup>8</sup> | Is afhankelijk van de grootte van het exemplaar<sup>8</sup> | 64,000 |
| Gelijktijdige [verbindingen van fout opsporing](../articles/app-service/troubleshoot-dotnet-visual-studio.md) per toepassing |1 |1 |1 |5 |5 |5 |
| App Service certificaten per abonnement<sup>9</sup>| Niet ondersteund | Niet ondersteund |10 |10 |10 |10 |
| </a> voor aangepaste domeinen per app |0 (alleen azurewebsites.net subdomein)|500 |500 |500 |500 |500 |
| SSL- [ondersteuning](../articles/app-service/configure-ssl-certificate.md) voor aangepaste domeinen |Niet ondersteund, Joker certificaat voor *. azurewebsites.net is standaard beschikbaar|Niet ondersteund, Joker certificaat voor *. azurewebsites.net is standaard beschikbaar|Onbeperkte SNI SSL verbindingen |Onbeperkt aantal SNI SSL en 1 IP SSL verbindingen |Onbeperkt aantal SNI SSL en 1 IP SSL verbindingen | Onbeperkt aantal SNI SSL en 1 IP SSL verbindingen|
| Hybride verbindingen per plan | | | 5 | 25 | 200 | 200 |
| Geïntegreerde load balancer | |X |X |X |X |X<sup>10</sup> |
| [Altijd aan](../articles/app-service/configure-common.md) | | |X |X |X |X |
| [Geplande back-ups](../articles/app-service/manage-backup.md) | | | | Geplande back-ups om de 2 uur, Maxi maal 12 back-ups per dag (hand matig + gepland) | Geplande back-ups elk uur, Maxi maal 50 back-ups per dag (hand matig + gepland) | Geplande back-ups elk uur, Maxi maal 50 back-ups per dag (hand matig + gepland) |
| [Automatisch schalen](../articles/app-service/manage-scale-up.md) | | | |X |X |X |
| [Webjobs](../articles/app-service/webjobs-create.md)<sup>11</sup> |X |X |X |X |X |X |
| Ondersteuning voor [Azure scheduler](https://azure.microsoft.com/services/scheduler/) | |X |X |X |X |X |
| [Eindpuntbewaking](../articles/app-service/web-sites-monitor.md) | | |X |X |X |X |
| [Staging-sleuven](../articles/app-service/deploy-staging-slots.md) | | | |5 |20 |20 |
| SLA | |  |99,95%|99,95%|99,95%|99,95%|  

<sup>1</sup> Apps en opslag quota zijn per App Service abonnement, tenzij anders vermeld.  
<sup>2</sup> Het werkelijke aantal apps dat u op deze computers kunt hosten, is afhankelijk van de activiteit van de apps, de grootte van de computer instanties en het bijbehorende resource gebruik.  
<sup>3</sup> Toegewezen instanties kunnen een andere grootte hebben. Zie [app service prijzen](https://azure.microsoft.com/pricing/details/app-service/)voor meer informatie.  
<sup>4</sup> Meer zijn toegestaan op aanvraag.  
<sup>5</sup> De opslag limiet is de totale inhouds grootte voor alle apps in hetzelfde app service-plan. De totale inhouds grootte van alle apps in alle app service-abonnementen in één resource groep en regio mag niet groter zijn dan 500 GB.  
<sup>6</sup> Deze resources worden beperkt door fysieke resources op de toegewezen instanties (de exemplaar grootte en het aantal exemplaren).  
<sup>7</sup> Als u een app in de basis-laag schaalt naar twee exemplaren, hebt u 350 gelijktijdige verbindingen voor elk van de twee exemplaren. Voor de Standard-laag en hierboven zijn er geen theoretische limieten voor web sockets, maar andere factoren kunnen het aantal websockets beperken. Zo zijn Maxi maal toegestane gelijktijdige aanvragen (gedefinieerd door `maxConcurrentRequestsPerCpu`): 7.500 per kleine VM, 15.000 per gemiddelde VM (7.500 x 2 kern geheugens) en 75.000 per grote VM (18.750 x 4 kern geheugens).  
<sup>8</sup> Het maximum aantal IP-verbindingen is per exemplaar en is afhankelijk van de instantie grootte: 1.920 per B1/S1/P1V2-exemplaar, 3.968 per B2/S2/P2V2-exemplaar, 8.064 per B3/S3/P3V2-exemplaar.  
<sup>9</sup> De quotum limiet van App Service Certificate per abonnement kan worden verhoogd via een ondersteunings aanvraag tot een maximum limiet van 200.  
<sup>10</sup> App Service Isolated Sku's kunnen intern worden verdeeld (ILB) met Azure Load Balancer, dus er is geen open bare verbinding via internet. Hierdoor moeten bepaalde functies van een ILB die is geïsoleerd van een App Service worden gebruikt op machines die rechtstreeks toegang hebben tot het eindpunt van het ILB-netwerk.  
<sup>11</sup> Aangepaste uitvoer bare bestanden en/of scripts op aanvraag, op basis van een schema of continu uitvoeren als een achtergrond taak binnen uw App Service-exemplaar. Altijd beschikbaar is vereist voor de continue uitvoering van WebJobs. Azure Scheduler Gratis of Standaard is vereist voor geplande WebJobs. Er is geen vooraf gedefinieerde limiet voor het aantal webtaken dat kan worden uitgevoerd in een App Service-exemplaar. Er zijn praktische limieten die afhankelijk zijn van wat de toepassings code probeert te doen.  
