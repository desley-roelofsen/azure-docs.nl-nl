---
title: De virtuele machine biedt een publicatie handleiding voor Azure Marketplace
description: In dit artikel worden de vereisten beschreven voor het publiceren van een virtuele machine en een gratis proef versie van een software die vanuit Marketplace kan worden geïmplementeerd.
services: Azure, Marketplace, Compute, Storage, Networking, Blockchain, Security
author: ellacroi
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: article
ms.date: 07/09/2018
ms.author: ellacroi
ms.openlocfilehash: 0f2047501b6c57b2bb98ff7e3c56498417644324
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/08/2019
ms.locfileid: "73818981"
---
# <a name="virtual-machine-offer-publishing-guide"></a>Publicatie handleiding voor de virtuele machine-aanbieding

Installatie kopieën van virtuele machines zijn een van de belangrijkste manieren om een oplossing te publiceren in azure Marketplace. Gebruik deze hand leiding om inzicht te krijgen in de vereisten voor deze aanbieding. 

Dit zijn transactie aanbiedingen die worden geïmplementeerd en gefactureerd via Marketplace. De aanroep van een actie die een gebruiker ziet is ' nu ophalen '.

## <a name="free-trial"></a>Gratis proefversie 

U kunt ervoor zorgen dat gebruikers uw aanbieding testen door gebruik te maken van beperkte termijn software licenties wanneer u het BYOL-facturerings model (uw eigen licentie) gebruikt. Hieronder vindt u de vereisten voor het implementeren van deze aanbieding. 

|Vereisten  |Details  |
|---------|---------|
|Gratis proef periode en proef ervaring     |   Uw klanten kunnen uw app gedurende een beperkte periode gratis uitproberen. Uw klanten zijn verplicht om licenties of abonnements kosten te betalen voor uw aanbieding. Uw klanten hoeven niet te betalen voor het onderliggende product of de onderhouds dienst van micro soft. Alle opties voor de proef versie worden geïmplementeerd in uw Azure-abonnement. U hebt uitsluitend de controle over de kosten optimalisatie en het beheer. U kunt kiezen voor een gratis proef versie of een interactieve demo. Wat u ook kiest, uw gratis proef versie moet klanten een vooraf ingestelde tijds duur bieden om uw aanbieding zonder extra kosten te kunnen proberen.|
|Eenvoudig Configureer bare, kant-en-klare oplossing    |  Uw app moet eenvoudig zijn en snel worden geconfigureerd en ingesteld.       |
|Beschik baarheid/uptime    |    Uw SaaS-app of-platform moet een uptime van ten minste 99,9% hebben.     |
|Azure Active Directory     |    Uw aanbieding moet Azure Active Directory (Azure AD) federatieve eenmalige aanmelding (SSO) (eenmalige aanmelding) (Azure AD Federated SSO) toestaan waarvoor toestemming is ingeschakeld.     |

## <a name="test-drive"></a>Test Drive

U implementeert een of meer virtuele machines via Infrastructure-as-a-Service (IaaS) of SaaS-apps (Software-as-a-Service). Een voor deel van de test drive publicatie optie is de geautomatiseerde inrichting van een virtuele machine of volledige oplossing, onder geleid door een door een partner gehoste rond leiding. Een test drive biedt een evaluatie zonder extra kosten voor uw klant. Uw klant hoeft geen bestaande Azure-klant te zijn om te kunnen samen werken met de proef ervaring. 

Neem contact met ons op op [amp-Testdrive](mailto:amp-testdrive@microsoft.com) om aan de slag te gaan. 

|Vereisten  |Details |
|---------|---------|
| U hebt een Marketplace-app   |    Een of meer virtuele machines via IaaS of SaaS.      |

## <a name="interactive-demo"></a>Interactieve demo

U geeft een begeleide ervaring van uw oplossing aan uw klanten door gebruik te maken van een interactieve demonstratie. Het voor deel van interactieve demo-publicatie optie is dat u een proef ervaring hebt zonder gecompliceerde inrichting van uw complexe oplossing. 

## <a name="virtual-machine-offer"></a>Aanbieding van de virtuele machine

Gebruik het type virtuele machine-aanbieding wanneer u een virtueel apparaat implementeert op het abonnement dat is gekoppeld aan uw klant. Vm's zijn volledig geschikt voor het gebruik van BYOL-licentie modellen (betalen per gebruik of uw eigen licentie). Micro soft host de commerce trans actie en factureert uw klant namens u. U krijgt het voor deel van het gebruik van de voorkeurs betalings relatie tussen uw klant en micro soft, inclusief alle Enter prise-overeenkomsten.

> [!NOTE]
> Op dit moment kunnen de monetaire toezeg gingen die zijn gekoppeld aan een Enterprise Agreement, worden gebruikt voor het Azure-gebruik van uw virtuele machine, maar niet op basis van de kosten voor software licenties.  
> 
> [!NOTE]
> U kunt de detectie en implementatie van uw virtuele machine beperken tot een specifieke set klanten door de installatie kopie en prijzen als een persoonlijke aanbieding te publiceren. Persoonlijke aanbiedingen bieden u de mogelijkheid om exclusieve aanbiedingen voor uw dichtstbijzijnde klanten te maken en om aangepaste software en voor waarden te leveren. Met de aangepaste voor waarden kunt u een groot aantal scenario's markeren, met inbegrip van veld LED-deals met gespecialiseerde prijzen en voor waarden, en vroegtijdige toegang tot beperkte release software. Met persoonlijke aanbiedingen kunt u specifieke prijzen of producten aan een beperkt aantal klanten geven door een nieuwe SKU te maken met deze gegevens.  
> *   Voor meer informatie over persoonlijke aanbiedingen gaat u naar de pagina met persoonlijke aanbiedingen op Azure Marketplace op [Azure.Microsoft.com/blog/private-offers-on-Azure-Marketplace](https://azure.microsoft.com/blog/private-offers-on-azure-marketplace).  

| Vereiste | Details |  
|:--- |:--- | 
| Facturering en meting | Uw virtuele machine moet de facturering per maand BYOL of betalen naar gebruik ondersteunen. |  
| Azure-compatibele virtuele harde schijf (VHD) | Vm's moeten zijn gebouwd op Windows of Linux. <ul> <li>Zie [Linux-distributies die zijn goedgekeurd op Azure](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros)voor meer informatie over het maken van een virtuele Linux-harde schijf.</li> <li>Zie [een virtuele harde schijf maken die compatibel is met Azure](./cloud-partner-portal/virtual-machine/cpp-create-vhd.md)voor meer informatie over het maken van een Windows VHD.</li> </ul> |  

>[!Note]
>Opt-in voor Cloud Solution Providers (CSP)-partner kanaal is nu beschikbaar.  Raadpleeg [Cloud Solution Providers](./cloud-solution-providers.md) voor meer informatie over het marketing gebruik van uw aanbieding via de micro soft CSP-partner kanalen.

## <a name="next-steps"></a>Volgende stappen

Als u dit nog niet hebt gedaan, 

- [Registreer](https://azuremarketplace.microsoft.com/sell) u in de Marketplace.

Als u bent geregistreerd en een nieuwe aanbieding maakt of aan een bestaand item werkt,

- [Meld u aan bij Cloud Partner-Portal](https://cloudpartner.azure.com) om uw aanbieding te maken of te volt ooien.
- Zie [aanbieding van de virtuele machine](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/virtual-machine/cpp-virtual-machine-offer) voor meer informatie.
