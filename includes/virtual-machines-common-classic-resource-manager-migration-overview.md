---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines
author: singhkays
ms.service: virtual-machines
ms.topic: include
ms.date: 04/25/2019
ms.author: kasing
ms.custom: include file
ms.openlocfilehash: 40da2016026c8a7e02d1b243a783d01559e8c197
ms.sourcegitcommit: ae8b23ab3488a2bbbf4c7ad49e285352f2d67a68
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/13/2019
ms.locfileid: "74005535"
---
In dit artikel wordt beschreven hoe u IaaS-resources (Infrastructure as a Service) migreert van het klassieke naar het Resource Manager-implementatie model en hoe u resources verbindt met de twee implementatie modellen die naast elkaar zijn opgenomen in uw abonnement met behulp van virtueel netwerk site-naar-site gateways. U kunt meer lezen over [Azure Resource Manager functies en voor delen](../articles/azure-resource-manager/resource-group-overview.md). 

## <a name="goal-for-migration"></a>Doel voor migratie
Met Resource Manager kunt u complexe toepassingen implementeren via sjablonen, virtuele machines configureren met behulp van VM-extensies en toegangs beheer en tagging van het netwerk. Azure Resource Manager bevat schaal bare, parallelle implementatie voor virtuele machines in beschikbaarheids sets. Het nieuwe implementatie model biedt ook een levenscyclus beheer van compute, netwerk en opslag onafhankelijk. Ten slotte is er een focus op het inschakelen van beveiliging standaard met het afdwingen van virtuele machines in een virtueel netwerk.

Bijna alle functies van het klassieke implementatie model worden ondersteund voor compute, netwerk en opslag onder Azure Resource Manager. Als u wilt profiteren van de nieuwe mogelijkheden van Azure Resource Manager, kunt u bestaande implementaties migreren vanuit het klassieke implementatie model.

## <a name="supported-resources-for-migration"></a>Ondersteunde bronnen voor migratie
Deze klassieke IaaS-bronnen worden tijdens de migratie ondersteund

* Virtuele machines
* Beschikbaarheidssets
* Cloudservices met virtuele machines
* Opslagaccounts
* Virtuele netwerken
* VPN-gateways
* Express route-gateways _(in hetzelfde abonnement als alleen Virtual Network)_
* Netwerkbeveiligingsgroepen
* Routetabellen
* Gereserveerde IP-adressen

## <a name="supported-scopes-of-migration"></a>Ondersteunde bereiken van migratie
Er zijn vier verschillende manieren om de migratie van compute-, netwerk-en opslag bronnen te volt ooien:

* [Migratie van virtuele machines (niet in een virtueel netwerk)](#migration-of-virtual-machines-not-in-a-virtual-network)
* [Migratie van virtuele machines (in een virtueel netwerk)](#migration-of-virtual-machines-in-a-virtual-network)
* [Migratie van opslag accounts](#migration-of-storage-accounts)
* [Migratie van niet-gekoppelde resources](#migration-of-unattached-resources)

### <a name="migration-of-virtual-machines-not-in-a-virtual-network"></a>Migratie van virtuele machines (niet in een virtueel netwerk)
In het Resource Manager-implementatie model wordt de beveiliging van uw toepassingen standaard afgedwongen. Alle vm's moeten zich in een virtueel netwerk in het Resource Manager-model bestaan. Het Azure-platform wordt opnieuw opgestart (`Stop`, `Deallocate`en `Start`) de virtuele machines als onderdeel van de migratie. U hebt twee opties voor de virtuele netwerken waarnaar de Virtual Machines wordt gemigreerd:

* U kunt het platform aanvragen om een nieuw virtueel netwerk te maken en de virtuele machine migreren naar het nieuwe virtuele netwerk.
* U kunt de virtuele machine migreren naar een bestaand virtueel netwerk in Resource Manager.

> [!NOTE]
> In dit migratie bereik zijn de bewerkingen voor het beheer vlak en de gegevenslaag bewerkingen mogelijk niet toegestaan gedurende een bepaalde periode tijdens de migratie.
>

### <a name="migration-of-virtual-machines-in-a-virtual-network"></a>Migratie van virtuele machines (in een virtueel netwerk)
Voor de meeste VM-configuraties wordt alleen de meta gegevens gemigreerd tussen het klassieke en het Resource Manager-implementatie model. De onderliggende Vm's worden uitgevoerd op dezelfde hardware, in hetzelfde netwerk en met dezelfde opslag. De bewerkingen op het beheer vlak zijn mogelijk niet toegestaan gedurende een bepaalde periode tijdens de migratie. Het gegevens vlak blijft echter gewoon werken. Dat wil zeggen dat uw toepassingen die worden uitgevoerd op virtuele machines (klassiek) geen uitval tijd ontstaan tijdens de migratie.

De volgende configuraties worden momenteel niet ondersteund. Als er in de toekomst ondersteuning wordt toegevoegd, kan het zijn dat sommige Vm's in deze configuratie downtime kunnen oplopen (ga dan stoppen, toewijzing ongedaan maken en VM-bewerkingen opnieuw starten).

* U hebt meer dan één beschikbaarheidsset in één Cloud service.
* U hebt een of meer beschikbaarheids sets en Vm's die zich niet in een beschikbaarheidsset bevinden in één Cloud service.

> [!NOTE]
> In dit migratie bereik is het beheer vlak mogelijk niet toegestaan gedurende een bepaalde periode tijdens de migratie. Voor bepaalde configuraties zoals eerder beschreven, treedt er downtime op het gegevens vlak op.
>

### <a name="migration-of-storage-accounts"></a>Migratie van opslag accounts
Als u naadloze migratie wilt toestaan, kunt u Resource Manager-Vm's implementeren in een klassiek opslag account. Met deze mogelijkheid kunnen Compute-en netwerk bronnen onafhankelijk van opslag accounts worden gemigreerd. Nadat u uw Virtual Machines en Virtual Network hebt gemigreerd, moet u uw opslag accounts migreren om het migratie proces te volt ooien.

Als uw opslag account geen gekoppelde schijven of Virtual Machines gegevens heeft en alleen blobs, bestanden, tabellen en wacht rijen bevat, kan de migratie naar Azure Resource Manager worden uitgevoerd als zelfstandige migratie zonder afhankelijkheden.

> [!NOTE]
> Het Resource Manager-implementatie model bevat niet het concept van klassieke installatie kopieën en schijven. Wanneer het opslag account is gemigreerd, zijn klassieke installatie kopieën en schijven niet zichtbaar in de stack van de Resource Manager, maar blijven de back-upvhd's in het opslag account.

In de volgende scherm afbeeldingen ziet u hoe u een klassiek opslag account bijwerkt naar een Azure Resource Manager Storage-account met behulp van Azure Portal:
1. Meld u aan bij de [Azure Portal](https://portal.azure.com).
2. Ga naar uw opslagaccount.
3. Klik in de sectie **instellingen** op **migreren naar arm**.
4. Klik op **valideren** om de haal baarheid van de migratie te bepalen.
5. Als de validatie is geslaagd, klikt u op **voorbereiden** om een gemigreerde opslag account te maken.
6. Typ **Ja** om de migratie te bevestigen en klik op **door voeren** om de migratie te volt ooien.

    ![Opslag account valideren](../includes/media/storage-account-upgrade-classic/storage-migrate-resource-manager-1.png)
    
    ![Opslag account voorbereiden](../includes/media/storage-account-upgrade-classic/storage-migrate-resource-manager-2.png)
    
    ![Migratie van opslag account volt ooien](../includes/media/storage-account-upgrade-classic/storage-migrate-resource-manager-3.png)

### <a name="migration-of-unattached-resources"></a>Migratie van niet-gekoppelde resources
Opslag accounts zonder gekoppelde schijven of Virtual Machines gegevens kunnen onafhankelijk van zichzelf worden gemigreerd.

Netwerk beveiligings groepen, route tabellen & gereserveerde Ip's die niet aan een Virtual Machines en virtuele netwerken zijn gekoppeld, kunnen ook onafhankelijk van zichzelf worden gemigreerd.

<br>

## <a name="unsupported-features-and-configurations"></a>Niet-ondersteunde functies en configuraties
Sommige functies en configuraties worden momenteel niet ondersteund. in de volgende secties worden onze aanbevelingen rond hen beschreven.

### <a name="unsupported-features"></a>Niet-ondersteunde functies
De volgende functies worden momenteel niet ondersteund. U kunt deze instellingen desgewenst verwijderen, de virtuele machines migreren en vervolgens de instellingen opnieuw inschakelen in het Resource Manager-implementatie model.

| Resourceprovider | Functie | Aanbeveling |
| --- | --- | --- |
| Compute | Niet-gekoppelde schijven van virtuele machines. | De VHD-blobs achter deze schijven worden gemigreerd wanneer het opslag account wordt gemigreerd. |
| Compute | Installatie kopieën van virtuele machines. | De VHD-blobs achter deze schijven worden gemigreerd wanneer het opslag account wordt gemigreerd. |
| Netwerk | Eind punt-Acl's. | Verwijder de eind punt-Acl's en voer de migratie opnieuw uit. |
| Netwerk | Application Gateway | Verwijder de Application Gateway voordat u begint met de migratie en maak de Application Gateway opnieuw nadat de migratie is voltooid. |
| Netwerk | Virtuele netwerken met VNet-peering. | Migreer Virtual Network naar Resource Manager en klik vervolgens op peer. Meer informatie over [VNet-peering](../articles/virtual-network/virtual-network-peering-overview.md). |

### <a name="unsupported-configurations"></a>Niet-ondersteunde configuraties
De volgende configuraties worden momenteel niet ondersteund.

| Service | Configuratie | Aanbeveling |
| --- | --- | --- |
| Resource Manager |Op rollen gebaseerde Access Control (RBAC) voor klassieke bronnen |Omdat de URI van de resources na de migratie is gewijzigd, is het raadzaam om de RBAC-beleids updates te plannen die na de migratie moeten plaatsvinden. |
| Compute |Meerdere subnetten die zijn gekoppeld aan een virtuele machine |De configuratie van het subnet bijwerken zodat deze slechts naar één subnet verwijst. Hiervoor moet u mogelijk een secundaire NIC (die verwijst naar een ander subnet) verwijderen van de virtuele machine en deze opnieuw koppelen nadat de migratie is voltooid. |
| Compute |Virtuele machines die deel uitmaken van een virtueel netwerk, maar waaraan geen expliciet subnet is toegewezen |U kunt de virtuele machine eventueel verwijderen. |
| Compute |Virtuele machines met waarschuwingen, beleid voor automatisch schalen |De migratie gaat door en deze instellingen worden verwijderd. Het wordt ten zeerste aanbevolen om uw omgeving te evalueren voordat u de migratie gaat uitvoeren. U kunt de instellingen voor waarschuwingen ook opnieuw configureren nadat de migratie is voltooid. |
| Compute |XML-VM-extensies (BGInfo 1. *, Visual Studio Debugger, Web Deploy en Remote Debug) |Dit wordt niet ondersteund. Het is raadzaam deze uitbrei dingen van de virtuele machine te verwijderen om de migratie voort te zetten of ze worden tijdens het migratie proces automatisch verwijderd. |
| Compute |Diagnostische gegevens over opstarten met Premium-opslag |Schakel de functie voor diagnostische gegevens over opstarten voor de virtuele machines uit voordat u doorgaat met de migratie. U kunt Diagnostische gegevens over opstarten opnieuw inschakelen in de Resource Manager-stack nadat de migratie is voltooid. Daarnaast moeten de blobs die worden gebruikt voor scherm afbeeldingen en seriële logboeken worden verwijderd, zodat u niet langer in rekening wordt gebracht voor deze blobs. |
| Compute | Cloud Services die web-of werk rollen bevatten | Dit wordt momenteel niet ondersteund. |
| Compute | Cloud Services die meer dan één beschikbaarheidsset of meerdere beschikbaarheids sets bevatten. |Dit wordt momenteel niet ondersteund. Verplaats de Virtual Machines naar dezelfde beschikbaarheidsset voordat u de migratie uitvoert. |
| Compute | VM met Azure Security Center extensie | Azure Security Center installeert automatisch extensies op uw Virtual Machines om de beveiliging te controleren en waarschuwingen te genereren. Deze uitbrei dingen worden meestal automatisch geïnstalleerd als het Azure Security Center beleid is ingeschakeld voor het abonnement. Als u de Virtual Machines wilt migreren, schakelt u het Security Center-beleid uit op het abonnement, waardoor de Security Center bewakings extensie uit de Virtual Machines wordt verwijderd. |
| Compute | VM met back-up-of momentopname uitbreiding | Deze uitbrei dingen worden geïnstalleerd op een virtuele machine die is geconfigureerd met de Azure Backup-service. Hoewel de migratie van deze Vm's niet wordt ondersteund, volgt u de instructies [hier](https://docs.microsoft.com/azure/virtual-machines/windows/migration-classic-resource-manager-faq#vault) om back-ups te houden die zijn uitgevoerd vóór de migratie.  |
| Netwerk |Virtuele netwerken die virtuele machines en web-en werk rollen bevatten |Dit wordt momenteel niet ondersteund. Verplaats de web-en werk rollen naar hun eigen Virtual Network voordat u de migratie uitvoert. Zodra de klassieke Virtual Network is gemigreerd, kan de gemigreerde Azure Resource Manager Virtual Network worden gekoppeld met de klassieke Virtual Network om Vergelijk bare configuratie te krijgen als voorheen.|
| Netwerk | Klassieke Express route-circuits |Dit wordt momenteel niet ondersteund. Deze circuits moeten naar Azure Resource Manager worden gemigreerd voordat de migratie van IaaS wordt gestart. Zie [ExpressRoute-circuits verplaatsen van het klassieke naar het Resource Manager-implementatie model](../articles/expressroute/expressroute-move.md)voor meer informatie.|
| Azure App Service |Virtuele netwerken die App Service omgevingen bevatten |Dit wordt momenteel niet ondersteund. |
| Azure HDInsight |Virtuele netwerken die HDInsight-services bevatten |Dit wordt momenteel niet ondersteund. |
| Micro soft Dynamics-levenscyclus Services |Virtuele netwerken die virtuele machines bevatten die worden beheerd door Dynamics levenscyclus Services |Dit wordt momenteel niet ondersteund. |
| Azure AD Domain Services |Virtuele netwerken die Azure AD Domain Services bevatten |Dit wordt momenteel niet ondersteund. |
| Azure API Management |Virtuele netwerken die Azure API Management-implementaties bevatten |Dit wordt momenteel niet ondersteund. Als u het IaaS VNET wilt migreren, wijzigt u het VNET van de API Management-implementatie. Dit is een niet-downtime-bewerking. |
