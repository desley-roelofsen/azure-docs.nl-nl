---
title: Prijzen van Azure Security Center lagen
description: 'Azure Security Center wordt aangeboden in twee lagen: gratis en standaard. Op deze pagina ziet u hoe u een upgrade uitvoert van gratis naar standaard.'
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 4d1364cd-7847-425a-bb3a-722cb0779f78
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/04/2019
ms.author: memildin
ms.openlocfilehash: 8ffb571d13270ced80426aee3575197cf95d3805
ms.sourcegitcommit: b5d59c6710046cf105236a6bb88954033bd9111b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/27/2019
ms.locfileid: "74559249"
---
# <a name="upgrade-to-security-centers-standard-tier-for-enhanced-security"></a>Voer een upgrade uit naar de Standard-laag van Security Center voor verbeterde beveiliging
Azure Security Center biedt geïntegreerd beveiligingsbeheer en geavanceerde bedreigingsbeveiliging voor werkbelastingen die worden uitgevoerd in Azure, on-premises en in andere clouds. Het biedt zicht baarheid en controle over hybride Cloud werkbelastingen, actieve beveiligingen die uw bloot stelling aan bedreigingen verminderen en intelligente detectie om u te helpen bij het snel zich ontwikkelen van Cyber aanvallen.

## <a name="pricing-tiers"></a>Prijscategorieën
Security Center wordt aangeboden in twee prijscategorieën:

- De **gratis** laag is ingeschakeld op alle Azure-abonnementen zodra u de Azure Security Center dash board in de Azure portal voor het eerst bezoekt of als u via een programma via API hebt ingeschakeld. De gratis laag biedt beveiligings beleid, doorlopende veiligheids beoordeling, en beschik bare beveiligings aanbevelingen om u te helpen uw Azure-resources te beveiligen.
- De laag **standaard** biedt een uitbrei ding op de mogelijkheden van de gratis laag voor werk belastingen die worden uitgevoerd in privé-en andere open bare Clouds, met een uniforme beveiligings beheer en bedreigings beveiliging in uw hybride Cloud werkbelastingen. De laag standaard biedt ook geavanceerde mogelijkheden voor het detecteren van bedreigingen, die gebruikmaken van ingebouwde gedrags analyses en machine learning om aanvallen te identificeren en gebruik te maken van anti-malware, toegangs-en toepassings besturings elementen van een dag om de bloot stelling aan netwerk aanvallen en schadelijke software te verminderen. grotere. U kunt de Standard-laag gratis uitproberen. Security Center Standard ondersteunt Azure-resources, waaronder Vm's, schaal sets voor virtuele machines, App Service, SQL-servers en opslag accounts. Als u Azure Security Center Standard hebt, kunt u zich niet meer aanmelden op basis van het resource type. 

De meeste beveiligings evaluaties van de gratis laag voor Vm's, evenals een groot deel van de standaard beveiligings waarschuwingen voor lagen, vereisen de installatie van de micro soft Monitoring Agent (MMA)-mogelijkheid. U kunt automatische inrichting inschakelen op Security Center om de agent automatisch te implementeren voor uw Azure-Vm's.

## <a name="try-standard-free-for-30-days"></a>Probeer gratis standaard 30 dagen
De Standard-laag is gratis gedurende de eerste 30 dagen. Als u aan het einde van 30 dagen wilt door gaan met het gebruik van de service, worden er automatisch kosten in rekening gebracht voor het gebruik.

U kunt een volledig Azure-abonnement upgraden naar de Standard-laag, die wordt overgenomen door alle resources in het abonnement.

De laag standaard ophalen:

1. Selecteer **prijzen & instellingen** in het hoofd menu van **Security Center** .
2. Selecteer het abonnement dat u wilt bijwerken naar Standard.
3. Selecteer **Prijscategorie**.
4. Selecteer **standaard** om bij te werken.
5. Klik op **Opslaan**.

(De prijzen in de afbeelding worden alleen gegeven voor demonstratie doeleinden) [prijzen voor![Security Center](media/security-center-pricing/pricing-tier-page.png)](media/security-center-pricing/pricing-tier-page.png#lightbox)

> [!NOTE]
> Als u alle Security Center-functies wilt inschakelen, moet u de prijscategorie Standaard toepassen op het abonnement met de toepasselijke virtuele machines. Als u de prijzen voor een werk ruimte configureert, kunt u niet just-in-time-VM-toegang, adaptieve toepassings besturings elementen en netwerk detecties voor Azure-resources.
>

## <a name="why-upgrade-to-standard"></a>Waarom upgraden naar Standard?
Security Center biedt verbeterde beveiliging en bedreigings beveiliging voor uw hybride Cloud werkbelastingen, waaronder:

- **Hybride beveiliging** : krijg een uniforme weer gave van beveiliging in al uw on-premises en Cloud werkbelastingen. Pas het beveiligings beleid toe en evalueer voortdurend de beveiliging van uw hybride Cloud werkbelastingen om te zorgen voor naleving van de beveiligings standaarden. Verzamel, zoek en analyseer beveiligings gegevens van meerdere bronnen, inclusief firewalls en andere partner oplossingen.
- **Geavanceerde detectie van bedreigingen** : Gebruik geavanceerde analyses en de Microsoft intelligent Security Graph om een rand te krijgen over het ontwikkelen van Cyber aanvallen. Maak gebruik van de ingebouwde gedragsanalyses en machine learning om aanvallen en zero-day exploits te identificeren. Controleer netwerken, computers en cloudservices op inkomende aanvallen en activiteiten na het schenden van de beveiliging. Stroomlijn het onderzoek met interactieve hulpprogramma's en contextuele bedreigingsinformatie.
- **Toegangs-en toepassings besturings elementen** : blok keer malware en andere ongewenste toepassingen door machine learning aanbevolen white list-aanbevelingen toe te passen die zijn afgestemd op uw specifieke workloads. Verminder de kwets baarheid van het netwerk met Just-in-time, gecontroleerde toegang tot beheer poorten op Azure-Vm's. Dit vermindert de bloot stelling aan brute kracht en andere netwerk aanvallen aanzienlijk.
- **Beveiligings functies voor containers** : Profiteer van beveiligings beheer en real-time detectie van bedreigingen in uw container omgevingen. Wanneer u de bron van de container registers inschakelt, kan het tot 12hrs duren totdat alle functies zijn ingeschakeld.


## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u de prijzen voor Security Center geïntroduceerd. Zie voor meer informatie over de verbeterde beveiliging en geavanceerde bedreigings beveiliging van de laag:

- [Geavanceerde detectie van bedreigingen](security-center-threat-report.md)
- [Just-in-time-VM-toegangs beheer](security-center-just-in-time.md)
- [Overzicht van container beveiliging](container-security.md)
- [Prijs informatie in de gewenste valuta en op basis van uw regio](https://azure.microsoft.com/pricing/details/security-center/)