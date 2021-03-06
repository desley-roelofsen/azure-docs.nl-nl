---
title: Veelgestelde vragen over Azure Network Watcher | Microsoft Docs
description: In dit artikel vindt u antwoorden op veelgestelde vragen over de Azure Network Watcher-service.
services: network-watcher
documentationcenter: na
author: damendo
manager: ''
editor: ''
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/10/2019
ms.author: damendo
ms.openlocfilehash: 3305590f2d8abf0d894bc1df42b84edcc96a2b2d
ms.sourcegitcommit: b4f201a633775fee96c7e13e176946f6e0e5dd85
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/18/2019
ms.locfileid: "72598225"
---
# <a name="frequently-asked-questions-faq-about-azure-network-watcher"></a>Veelgestelde vragen over Azure Network Watcher
De [Azure Network Watcher](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview) -service biedt een reeks hulpprogram ma's voor het bewaken, diagnosticeren, weer geven van metrische gegevens en het in-of uitschakelen van Logboeken voor bronnen in een virtueel Azure-netwerk. In dit artikel vindt u antwoorden op veelgestelde vragen over de service.

## <a name="general"></a>Algemeen

### <a name="what-is-network-watcher"></a>Wat is Network Watcher?
Network Watcher is ontworpen om de netwerk status van IaaS-onderdelen (Infrastructure-as-a-Service) te controleren en te herstellen, waaronder Virtual Machines, virtuele netwerken, toepassings gateways, load balancers en andere bronnen in een virtueel Azure-netwerk. Het is geen oplossing voor het bewaken van de PaaS-infra structuur (platform-as-a-Service) of voor het ophalen van Web/Mobile-analyses.

### <a name="what-tools-does-network-watcher-provide"></a>Welke hulpprogram ma's Network Watcher bieden?
Network Watcher biedt drie belang rijke sets mogelijkheden
* Controleren
  * In de [topologie weergave](https://docs.microsoft.com/azure/network-watcher/view-network-topology) ziet u de resources in uw virtuele netwerk en de relaties daartussen.
  * Met [verbindings monitor](https://docs.microsoft.com/azure/network-watcher/connection-monitor) kunt u de connectiviteit en latentie tussen een virtuele machine en een andere netwerk bron bewaken.
  * Met [netwerk prestatie meter](https://docs.microsoft.com/azure/azure-monitor/insights/network-performance-monitor) kunt u connectiviteit en latentie bewaken over hybride netwerk architecturen, Expressroute-circuits en service/toepassings eindpunten.  
* Diagnostics
  * Met [IP-stroom controle](https://docs.microsoft.com/azure/network-watcher/network-watcher-ip-flow-verify-overview) kunt u problemen met verkeer filteren op een VM-niveau detecteren.
  * [Volgende hop](https://docs.microsoft.com/azure/network-watcher/network-watcher-next-hop-overview) helpt u bij het controleren van verkeers routes en het detecteren van routerings problemen.
  * [Met verbindings problemen oplossen](https://docs.microsoft.com/azure/network-watcher/network-watcher-connectivity-portal) kunt u een eenmalige verbinding en latentie controle tussen een virtuele machine en een andere netwerk bron.
  * Met [pakket opname](https://docs.microsoft.com/azure/network-watcher/network-watcher-packet-capture-overview) kunt u al het verkeer op een VM in uw virtuele netwerk vastleggen.
  * [Met VPN-problemen oplossen](https://docs.microsoft.com/azure/network-watcher/network-watcher-troubleshoot-overview) voert u meerdere diagnostische controles uit op uw VPN-gateways en verbindingen om problemen met de fout op te lossen.
* Logboekregistratie
  * Met [NSG-stroom logboeken](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-overview) kunt u al het verkeer in uw [netwerk beveiligings groepen registreren (nsg's)](https://docs.microsoft.com/azure/virtual-network/security-overview)
  * [Traffic Analytics](https://docs.microsoft.com/azure/network-watcher/traffic-analytics) verwerkt de gegevens van uw NSG-stroom logboek om uw netwerk verkeer te visualiseren, op te vragen, te analyseren en te begrijpen.


Zie de [overzichts pagina van Network Watcher](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview)voor meer informatie.


### <a name="how-does-network-watcher-pricing-work"></a>Hoe werkt Network Watcher prijzen?
Ga naar de [pagina met prijzen](https://azure.microsoft.com/pricing/details/network-watcher/) voor Network Watcher onderdelen en hun prijzen.

### <a name="which-regions-is-network-watcher-available-in"></a>In welke regio's is Network Watcher beschikbaar?
U kunt de nieuwste regionale Beschik baarheid bekijken op de [pagina Beschik baarheid van Azure-service](https://azure.microsoft.com/global-infrastructure/services/?products=network-watcher)

### <a name="what-are-resource-limits-on-network-watcher"></a>Wat zijn de limieten voor resources op Network Watcher?
Zie de pagina met [service beperkingen](https://docs.microsoft.com/azure/azure-subscription-service-limits#network-watcher-limits) voor alle limieten.  

### <a name="why-is-only-one-instance-of-network-watcher-allowed-per-region"></a>Waarom is er slechts één exemplaar van Network Watcher per regio toegestaan?
Network Watcher hoeft alleen maar één keer te worden ingeschakeld voor een abonnement, maar dit is geen service limiet.

## <a name="nsg-flow-logs"></a>NSG-stroom logboeken

### <a name="what-does-nsg-flow-logs-do"></a>Wat gebeurt er met NSG-stroom logboeken?
Azure-netwerk bronnen kunnen worden gecombineerd en beheerd via [netwerk beveiligings groepen (nsg's)](https://docs.microsoft.com/azure/virtual-network/security-overview). Met NSG-stroom Logboeken kunt u 5-tuple-stroom gegevens registreren over al het verkeer via uw Nsg's. De onbewerkte stroom logboeken worden naar een Azure Storage-account geschreven, waar ze verder kunnen worden verwerkt, geanalyseerd, opgevraagd of geëxporteerd als dat nodig is.

### <a name="are-there-any-caveats-to-using-nsg-flow-logs"></a>Zijn er aanvullende opmerkingen voor het gebruik van NSG-stroom logboeken?
Er zijn geen vereisten voor het gebruik van NSG-stroom Logboeken. Er zijn echter twee beperkingen
- **Service-eind punten mogen niet aanwezig zijn in uw VNET**: NSG-stroom logboeken worden verzonden van agents op uw Vm's naar opslag accounts. Vandaag kunt u echter alleen logboeken rechtstreeks naar opslag accounts verzenden en een service-eind punt dat is toegevoegd aan uw VNET, niet gebruiken.

- **Er mag geen firewall worden**gebruikt voor het opslag account: vanwege interne beperkingen moeten opslag accounts toegankelijk zijn via het open bare Internet voor NSG-stroom Logboeken. Verkeer wordt nog steeds intern gerouteerd door Azure en er worden geen extra kosten in rekening gebracht.

Raadpleeg de volgende twee vragen voor instructies over het omzeilen van deze problemen. Beide beperkingen worden naar verwachting verholpen met Jan 2020.

### <a name="how-do-i-use-nsg-flow-logs-with-service-endpoints"></a>Hoe kan ik NSG-stroom Logboeken gebruiken met Service-eind punten?

*Optie 1: NSG-stroom logboeken opnieuw configureren voor het verzenden naar Azure Storage account zonder VNET-eind punten*

* Subnetten met eindpunten zoeken:

    - Ga in de Azure-portal naar **Resourcegroepen** in de algemene zoekfunctie bovenaan
    - Navigeer naar de resourcegroep met de NSG waarmee u werkt
    - Gebruik de tweede vervolg keuzelijst om te filteren op type en **virtuele netwerken** te selecteren
    - Klik op het virtuele netwerk met de service-eindpunten
    - Selecteer onder **Instellingen** in het linkerdeelvenster de optie **Service-eindpunt**
    - Noteer de subnetten waarvoor **Microsoft.Storage** is ingeschakeld

* Service-eind punten uitschakelen:

    - Verdergaand op het bovenstaande selecteert u onder **Instellingen** in het linkerdeelvenster de optie **Service-eindpunt**
    * Klik op het subnet met de service-eindpunten
    - Schakel in de sectie **Service-eindpunten**, onder **Services**, de optie **Microsoft.Storage** uit

U kunt de Storage-logboeken na enkele minuten controleren. U ziet dan een bijgewerkte tijdstempel of een nieuw JSON-bestand.

*Optie 2: NSG-stroom Logboeken uitschakelen*

Als de Microsoft.Storage-service-eindpunten een vereiste zijn, moet u de NSG-stroomlogboeken uitschakelen.

### <a name="how-do-i-disable-the--firewall-on-my-storage-account"></a>De firewall van mijn opslag account Hoe kan ik uitschakelen?

Dit probleem wordt opgelost door alle netwerken in te scha kelen voor toegang tot het opslag account:

* U vindt de naam van het opslagaccount door naar de NSG te gaan op de [overzichtspagina voor NSG-stroomlogboeken](https://ms.portal.azure.com/#blade/Microsoft_Azure_Network/NetworkWatcherMenuBlade/flowLogs)
* Navigeer naar het opslagaccount door de naam van het opslagaccount te typen in de algemene zoekfunctie in de portal
* Selecteer in de sectie **INSTELLINGEN** de optie **Firewalls en virtuele netwerken**
* Selecteer **Alle netwerken** en sla dit op. Als deze optie al is geselecteerd, is er geen wijziging nodig.  

### <a name="what-is-the-difference-between-flow-logs-versions-1--2"></a>Wat is het verschil tussen stroom logboeken versie 1 & 2?
Stroom logboeken versie 2 introduceert het concept van de *stroom status* & slaat informatie op over verzonden bytes en pakketten. [Meer informatie](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-overview#log-file).

## <a name="next-steps"></a>Volgende stappen
 - Ga naar de [pagina overzicht](https://docs.microsoft.com/azure/network-watcher/) van de documentatie voor enkele zelf studies om aan de slag te gaan met Network Watcher.
