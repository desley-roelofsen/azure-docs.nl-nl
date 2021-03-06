---
title: Service quota's en limieten-Azure Batch | Microsoft Docs
description: Meer informatie over standaard Azure Batch quota's, limieten en beperkingen, en het aanvragen van quotum verhogingen
services: batch
documentationcenter: ''
author: laurenhughes
manager: gwallace
editor: ''
ms.assetid: 28998df4-8693-431d-b6ad-974c2f8db5fb
ms.service: batch
ms.workload: big-compute
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 08/13/2019
ms.author: lahugh
ms.custom: seodec18
ms.openlocfilehash: 8b089a1b32ee70479072522372c060713108957c
ms.sourcegitcommit: 7f6d986a60eff2c170172bd8bcb834302bb41f71
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/27/2019
ms.locfileid: "71350094"
---
# <a name="batch-service-quotas-and-limits"></a>Quota en limieten voor Batch-service

Net als bij andere Azure-Services gelden er limieten voor bepaalde resources die zijn gekoppeld aan de batch-service. Veel van deze limieten zijn standaard quota's die worden toegepast door Azure op het niveau van de abonnement of het account. In dit artikel worden deze standaard waarden beschreven en wordt uitgelegd hoe u quotum verhogingen kunt aanvragen.

Houd bij het ontwerpen en opschalen van uw batch-workloads de volgende quota's in acht. Als uw pool bijvoorbeeld niet het doel aantal reken knooppunten bereikt dat u hebt opgegeven, hebt u mogelijk de kern quotum limiet voor uw batch-account bereikt.

U kunt in één Batch-account meerdere Batch-workloads uitvoeren of uw workloads verdelen over Batch-accounts in hetzelfde abonnement maar in verschillende Azure-regio's.

Als u in batch productie werkbelastingen wilt uitvoeren, moet u mogelijk een of meer van de quota's boven de standaard waarde verhogen. Als u een quotum wilt verhogen, kunt u gratis een online [klant ondersteunings aanvraag](#increase-a-quota) openen.

## <a name="resource-quotas"></a>Resourcequota

Een quotum is een tegoed is besteed, niet een capaciteit gegarandeerd. Neem contact op met de ondersteuning van Azure als u de capaciteits behoeften op grote schaal hebt.

Houd er ook rekening mee dat quota's geen gegarandeerde waarden zijn. Quota kunnen variëren op basis van wijzigingen van de batch-service of een gebruikers aanvraag om een quotum waarde te wijzigen.

[!INCLUDE [azure-batch-limits](../../includes/azure-batch-limits.md)]

### <a name="cores-quotas-in-user-subscription-mode"></a>Kernen quota's in de modus gebruikers abonnement

Als u een batch-account hebt gemaakt met de groeps toewijzings modus ingesteld op **gebruikers abonnement**, worden quota anders toegepast. In deze modus worden batch-Vm's en andere resources rechtstreeks in uw abonnement gemaakt wanneer er een groep wordt gemaakt. De quota voor de Azure Batch-kernen zijn niet van toepassing op een account dat in deze modus wordt gemaakt. In plaats daarvan worden de quota's in uw abonnement voor regionale reken kernen en andere resources toegepast. Meer informatie over deze quota's vindt u in [Azure-abonnement en service limieten, quota's en beperkingen](../azure-subscription-service-limits.md).

## <a name="pool-size-limits"></a>Limieten voor groeps grootte

Limieten voor groeps grootte worden ingesteld door de batch-service. In tegens telling tot [resource quota](#resource-quotas)kunnen deze waarden niet worden gewijzigd. Alleen groepen met communicatie tussen knoop punten en aangepaste installatie kopieën hebben andere beperkingen dan het standaard quotum.

| **Resource** | **Maximumaantal** |
| --- | --- |
| **Reken knooppunten in de [groep communicatie tussen knoop](batch-mpi.md) punten**  ||
| Pool toewijzings modus Batch-service | 100 |
| Groeps toewijzings modus Batch-abonnement | 80 |
| **Reken knooppunten in [de groep die zijn gemaakt met een beheerde installatie kopie bron](batch-custom-images.md)** <sup>1</sup> ||
| Toegewezen knooppunten | 2000 |
| Knooppunten met lage prioriteit | 1000 |

<sup>1</sup> voor groepen waarvoor geen communicatie tussen knoop punten is ingeschakeld.

## <a name="other-limits"></a>Andere limieten

Aanvullende limieten die zijn ingesteld door de batch-service. In tegens telling tot [resource quota](#resource-quotas)kunnen deze waarden niet worden gewijzigd.

| **Resource** | **Maximumaantal** |
| --- | --- |
| [Gelijktijdige taken](batch-parallel-node-tasks.md) per reken knooppunt | 4 x het aantal knooppunt kernen |
| [Toepassingen](batch-application-packages.md) per batch-account | 20 |
| Toepassingspakketten per toepassing | 40 |
| Toepassings pakketten per pool | 10 |
| Maximale levens duur van taken | 180 dagen<sup>1</sup> |
| [Koppelingen](virtual-file-mount.md) per Compute-knoop punt | 10 |

<sup>1</sup> de maximale levens duur van een taak, van wanneer deze wordt toegevoegd aan de taak wanneer deze is voltooid, is 180 dagen. Voltooide taken blijven zeven dagen geldig; gegevens voor taken die niet binnen de maximale levens duur zijn voltooid, zijn niet toegankelijk.

## <a name="view-batch-quotas"></a>Batch quota's weer geven

Bekijk uw batch-account quota's in het [Azure Portal][portal].

1. Selecteer **batch-accounts** in de portal en selecteer vervolgens het batch-account waarin u bent geïnteresseerd.
1. Selecteer **quota's** in het menu van het batch-account.
1. De quota's weer geven die momenteel zijn toegepast op het batch-account

    ![Quota voor batch-account][account_quotas]

## <a name="increase-a-quota"></a>Een quotum verhogen

Volg deze stappen om een quotum verhoging aan te vragen voor uw batch-account of uw abonnement met behulp van de [Azure Portal][portal]. Het type quotum toename is afhankelijk van de pool toewijzings modus van uw batch-account. Als u een quotum toename wilt aanvragen, moet u de VM-reeks toevoegen waarvoor u het quotum wilt verhogen. Wanneer de quota toename wordt toegepast, wordt deze toegepast op alle reeksen Vm's.

### <a name="increase-cores-quota-in-batch"></a>Het quotum voor kernen in batch verhogen 

1. Selecteer de tegel **Help + ondersteuning** in het dash board van de portal of het vraag teken ( **?** ) in de rechter bovenhoek van de portal.
1. Selecteer**basis principes** **nieuwe ondersteunings aanvraag** > .
1. In de **basis beginselen**:
   
    a.  > Limieten voor de service en abonnementen van het probleem type **(quota's)**
   
    b. Selecteer uw abonnement.
   
    c.  > **Batch** voor quotum type
      
    Selecteer **Volgende**.
    
1. In **Details**:
      
    a. Geef in **Details**opgeven de locatie, het quotum type en het batch-account op.
    
    ![Verhoging van batch quotum][quota_increase]

    De volgende quota typen zijn beschikbaar:

    * **Per batch-account**  
        Waarden die specifiek zijn voor één batch-account, inclusief specifieke kernen met een lage prioriteit en het aantal taken en Pools.
        
    * **Per regio**  
        Waarden die van toepassing zijn op alle batch-accounts in een regio en bevat het aantal batch-accounts per regio per abonnement.

    Quota met lage prioriteit is één waarde in alle VM-reeksen. Als u beperkte Sku's nodig hebt, moet u kernen **met een lage prioriteit** selecteren en de te aanvragen VM-families toevoegen.

    b. Selecteer een **Ernst** op basis van uw [bedrijfs impact][support_sev].

    Selecteer **Volgende**.

1. Bij **contact gegevens**:
   
    a. Selecteer een **Voorkeurs methode voor contact personen**.
   
    b. Controleer de vereiste contact gegevens en geef deze op.
   
    Selecteer **maken** om de ondersteunings aanvraag in te dienen.

Zodra u uw ondersteunings aanvraag hebt ingediend, neemt de ondersteuning van Azure contact met u op. Quota aanvragen kunnen binnen een paar minuten of Maxi maal twee werk dagen worden voltooid.

## <a name="related-quotas-for-vm-pools"></a>Gerelateerde quota's voor VM-groepen

Batch-Pools in de virtuele-machine configuratie die is geïmplementeerd in een virtueel Azure-netwerk, worden automatisch extra Azure-netwerk resources toegewezen. De volgende resources zijn nodig voor elk 50 pool-knoop punten in een virtueel netwerk:

* Eén [netwerk beveiligings groep](../virtual-network/security-overview.md#network-security-groups)
* Eén [openbaar IP-adres](../virtual-network/virtual-network-ip-addresses-overview-arm.md)
* Een [Load Balancer](../load-balancer/load-balancer-overview.md)

Deze resources worden toegewezen in het abonnement met het virtuele netwerk dat is geleverd bij het maken van de batch-pool. De beperkingen die voor deze resources gelden, worden bepaald door de [resourcequota](../azure-subscription-service-limits.md) van het abonnement. Als u implementaties van grote groepen plant in een virtueel netwerk, controleert u de quota van het abonnement voor deze resources. Vraag, indien nodig, een verhoging van de Azure Portal aan door **Help en ondersteuning**te selecteren.


## <a name="related-topics"></a>Verwante onderwerpen
* [Een Azure Batch-account maken met behulp van de Azure Portal](batch-account-create-portal.md)
* [Overzicht van Azure Batch-functies](batch-api-basics.md)
* [Azure subscription and service limits, quotas, and constraints](../azure-subscription-service-limits.md) (Limieten, quota's en beperkingen voor het Azure-abonnement en de Azure-service)

[portal]: https://portal.azure.com
[portal_classic_increase]: https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/
[support_sev]: https://aka.ms/supportseverity

[account_quotas]: ./media/batch-quota-limit/accountquota_portal.png
[quota_increase]: ./media/batch-quota-limit/quota-increase.png
