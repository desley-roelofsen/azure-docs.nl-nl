---
title: Azure DDoS Protection Standard-overzicht | Microsoft Docs
description: Meer informatie over de Azure DDoS Protection-Service.
services: virtual-network
documentationcenter: na
author: KumudD
manager: twooley
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/13/2018
ms.author: kumud
ms.openlocfilehash: ef2319f18b6df15fd7f33e9344e8506f853f47e6
ms.sourcegitcommit: 6eecb9a71f8d69851bc962e2751971fccf29557f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/17/2019
ms.locfileid: "72532222"
---
# <a name="azure-ddos-protection-standard-overview"></a>Azure DDoS Protection standaard overzicht

DDoS-aanvallen (Distributed Denial of Service-aanvallen) vormen een van de grootste beschikbaarheids- en beveiligingsproblemen voor klanten die hun toepassingen verplaatsen naar de cloud. Met een DDoS-aanval wordt geprobeerd de resources van een toepassing uit te putten, waardoor de toepassing niet meer beschikbaar is voor legitieme gebruikers. DDoS-aanvallen kunnen worden gericht op elk eindpunt dat openbaar bereikbaar is via internet.

Azure DDoS Protection, in combi natie met aanbevolen procedures voor het ontwerpen van toepassingen, biedt verdediging tegen DDoS-aanvallen. Azure DDoS Protection biedt de volgende service lagen:

- **Basic**: automatisch ingeschakeld als onderdeel van het Azure-platform. Controle over altijd verkeer en real-time beperking van veelvoorkomende aanvallen op netwerk niveau, bieden dezelfde verdedigings die worden gebruikt door de onlineservices van micro soft. De volledige schaal van het wereld wijde netwerk van Azure kan worden gebruikt voor het distribueren en beperken van het aanvals verkeer tussen regio's. Beveiliging is beschikbaar voor IPv4-en IPv6 Azure [open bare IP-adressen](virtual-network-public-ip-address.md).
- **Standard**: biedt extra mogelijkheden voor risico beperking ten opzichte van de Basic-servicelaag die specifiek zijn afgestemd op Azure Virtual Network-Resources. DDoS Protection Standard is eenvoudig in te scha kelen en vereist geen wijzigingen in de toepassing. Beveiligings beleid wordt afgestemd met behulp van toegewezen verkeers bewaking en machine learning-algoritmen. Beleids regels worden toegepast op open bare IP-adressen die zijn gekoppeld aan resources die zijn geïmplementeerd in virtuele netwerken, zoals Azure Load Balancer, Azure-toepassing gateway en Azure Service Fabric instances, maar deze beveiliging is niet van toepassing op App Service omgevingen. Realtime-telemetrie is beschikbaar via Azure Monitor weergaven tijdens een aanval en voor geschiedenis. Uitgebreide aanvals beperkings analyse zijn beschikbaar via Diagnostische instellingen. U kunt de beveiliging van de toepassingslaag toevoegen via de [Azure-toepassing gateway Web Application firewall](../application-gateway//application-gateway-web-application-firewall-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) of door een firewall van een derde partij te installeren vanuit Azure Marketplace. Beveiliging is beschikbaar voor IPv4-en IPv6 Azure [open bare IP-adressen](virtual-network-public-ip-address.md).

|Functie                                         |DDoS Protection Basic                 |DDoS Protection standaard                      |
|------------------------------------------------|--------------------------------------|----------------------------------------------|
|Bewaking van actief verkeer & altijd op detectie |Ja                                   |Ja                                           |
|Automatische aanvals oplossingen                    |Ja                                   |Ja                                           |
|Beschikbaarheids garantie                          |Azure-regio                          |Toepassing                                   |
|Risico beperkings beleid                             |Afgestemd op volume van Azure Traffic Region |Afgestemd op volume van toepassings verkeer          |
|Metrische gegevens en waarschuwingen                                |Nee                                    |Realtime aanvals metrieken & Diagnostische logboeken via Azure monitor                                 |
|Risico beperkings rapporten                              |Nee                                    |Rapporten voor risico beperking na aanvallen                |
|Stroom logboeken voor risico beperking                            |Nee                                    |NRT-logboek stroom voor SIEM-integratie           |
|Aanpassingen migratie beleid                 |Nee                                    |DDoS-experts benaderen                           |
|Ondersteuning                                         |Beste poging                           |Toegang tot DDoS-experts tijdens een actieve aanval|
|SLA                                             |Azure-regio                          |& Kosten beveiliging van de toepassings garantie       |
|Prijzen                                         |Gratis                                  |Maandelijks & gebruik op basis van                         |

## <a name="types-of-ddos-attacks-that-ddos-protection-standard-mitigates"></a>Typen DDoS-aanvallen die standaard oplossingen DDoS Protection

DDoS Protection Standard kan de volgende typen aanvallen beperken:

- **Volumetrische aanvallen**: het doel van de aanval is het flooden van de netwerklaag met een aanzienlijke hoeveelheid schijnbaar betrouwbaar verkeer. Dit omvat UDP-flooden, versterking van stromen en andere vervalste pakket stromen. DDoS Protection Standard verkleint deze potentiële multi-Gigabyte-aanvallen door ze te absorberen en te reinigen, met de wereld wijde schaal van Azure automatisch.
- **Protocol aanvallen**: deze aanvallen genereren een doel dat niet toegankelijk is door een zwakte te misbruiken in de Layer 3-en Layer 4-protocol stack. Dit omvat, SYN-flood-aanvallen, reflectie aanvallen en andere protocol aanvallen. DDoS Protection Standard verkleint deze aanvallen, onderscheidt zich van schadelijk en betrouwbaar verkeer door interactie met de client en het blok keren van schadelijk verkeer. 
- **Resource (Application) Layer-aanvallen**: deze aanvallen richten op webtoepassingen, om de overdracht van gegevens tussen hosts te verstoren. De aanvallen omvatten schendingen van het HTTP-protocol, SQL-injectie, cross-site scripting en andere Layer 7-aanvallen. Gebruik de Azure [Application Gateway Web Application firewall](../application-gateway/application-gateway-web-application-firewall-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json)met DDoS Protection Standard om bescherming tegen deze aanvallen te bieden. Er zijn ook Web Application Firewall aanbiedingen van derden beschikbaar in [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps?page=1&search=web%20application%20firewall).

DDoS Protection Standard beveiligt bronnen in een virtueel netwerk, met inbegrip van open bare IP-adressen die zijn gekoppeld aan virtuele machines, load balancers en toepassings gateways. In combi natie met de Application Gateway Web Application Firewall kan DDoS Protection Standard de mogelijkheid bieden om te voorzien in volledige laag 3 en laag 7.

## <a name="ddos-protection-standard-features"></a>DDoS Protection standaard functies

![DDoS-functionaliteit](./media/ddos-protection-overview/ddosfeatures.png)

DDoS Protection standaard functies zijn:

- **Systeem eigen platform integratie:** Systeem eigen geïntegreerd in Azure. Bevat configuratie via de Azure Portal. DDoS Protection standaard begrijpt uw resources en resource configuratie.
- **Beveiliging op basis van een bocht:** Vereenvoudigde configuratie beveiligt onmiddellijk alle resources in een virtueel netwerk zodra DDoS Protection standaard is ingeschakeld. Er is geen interventie-of gebruikers definitie vereist. DDoS Protection Standard onmiddellijk en vermindert de aanval automatisch, zodra deze is gedetecteerd.
- **Bewaking over altijd verkeer:** De patronen van uw toepassings verkeer worden 24 uur per dag, 7 dagen per week gecontroleerd en er wordt gezocht naar indica toren van DDoS-aanvallen. De beperking wordt toegepast wanneer het beveiligings beleid wordt overschreden.
- **Adaptieve afstemming:** Intelligent verkeer profile ring leert het verkeer van uw toepassing gedurende een bepaalde periode en selecteert en werkt het profiel dat het meest geschikt is voor uw service. Het profiel wordt aangepast naarmate het verkeer na verloop van tijd verandert.
- **Beveiliging met meerdere lagen:** Biedt volledige stack DDoS-beveiliging, wanneer deze met een Web Application Firewall wordt gebruikt.
- **Uitgebreide beperkings schaal:** Meer dan 60 verschillende typen aanvallen kunnen worden gereduceerd, met globale capaciteit om te beschermen tegen de grootste bekende DDoS-aanvallen.
- **Aanvals analyse:** Ontvang gedetailleerde rapporten in stappen van vijf minuten tijdens een aanval en een volledig overzicht nadat de aanval is beëindigd. Stroom logboeken voor risico beperking van de stroom naar een SIEM-systeem (offline Security Information and Event Management) voor bijna realtime bewaking tijdens een aanval.
- **Maat staven voor aanvallen:** Een overzicht van de metrische gegevens van elke aanval is toegankelijk via Azure Monitor.
- **Waarschuwing voor aanvallen:** Waarschuwingen kunnen worden geconfigureerd bij het starten en stoppen van een aanval en over de duur van de aanval, met behulp van ingebouwde aanvals waarden. Waarschuwingen worden geïntegreerd in uw operationele software, zoals Microsoft Azure controle logboeken, Splunk, Azure Storage, E-mail en de Azure Portal.
- **Kosten garantie:** Service-tegoeden voor gegevens overdracht en toepassingen voor gedocumenteerde DDoS-aanvallen.

## <a name="ddos-protection-standard-mitigation"></a>DDoS Protection standaard beperking

DDoS Protection Standard bewaakt het werkelijke verkeers gebruik en vergelijkt deze met de drempel waarden die zijn gedefinieerd in het DDoS-beleid. Wanneer de drempel waarde voor verkeer wordt overschreden, wordt de DDoS-beperking automatisch gestart. Wanneer het verkeer onder de drempel waarde komt, wordt de risico beperking verwijderd.

![Oplossing](./media/ddos-protection-overview/mitigation.png)

Tijdens de risico beperking wordt het verkeer dat wordt verzonden naar de beveiligde bron, omgeleid door de DDoS Protection-Service en worden er diverse controles uitgevoerd, zoals de volgende controles:

- Zorg ervoor dat de pakketten voldoen aan de Internet specificaties en niet zijn beschadigd.
- Communiceer met de client om te bepalen of het verkeer mogelijk een vervalst pakket is (bijvoorbeeld: SYN auth of SYN cookie of door een pakket voor de bron te verwijderen om het opnieuw te verzenden).
- Aantal pakketten beperken als er geen andere afdwing methode kan worden uitgevoerd.

Met DDoS Protection wordt het verkeer van aanvallen geblokkeerd en wordt het resterende verkeer doorgestuurd naar de bestemming. Binnen een paar minuten na de aanval wordt u hiervan op de hoogte gebracht met behulp van Azure Monitor metrische gegevens. Door logboek registratie in DDoS Protection standaard-telemetrie te configureren, kunt u de logboeken schrijven naar beschik bare opties voor toekomstige analyse. Metrische gegevens in Azure Monitor voor DDoS Protection standaard worden 30 dagen bewaard.

Micro soft heeft een samen werking met [BreakingPoint-Cloud](https://www.ixiacom.com/products/breakingpoint-cloud) voor het bouwen van een interface waarmee u verkeer kunt genereren op basis van DDoS Protection open bare IP-adressen voor simulaties. Met de Cloud simulatie onderbrekings punt kunt u:

- Valideer hoe Microsoft Azure DDoS Protection Standard uw Azure-resources beveiligt tegen DDoS-aanvallen
- Optimaliseer uw respons proces van uw incidenten door onder DDoS-aanval
- Conformiteit document DDoS
- Uw netwerk beveiligings teams trainen

## <a name="next-steps"></a>Volgende stappen

- [DDoS Protection Standard configureren](manage-ddos-protection.md)
