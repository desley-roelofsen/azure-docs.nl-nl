---
title: Azure Databricks-korting bij aankoop vooraf toepassen
description: Lees hoe Azure Databricks-korting bij aankoop vooraf wordt toegepast op uw gebruik.
services: billing
author: yashesvi
manager: yashar
ms.service: cost-management-billing
ms.topic: conceptual
ms.date: 10/01/2019
ms.author: banders
ms.openlocfilehash: 813bb27a04b86e63ed491f2d7fbbc25281f79c4c
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/20/2019
ms.locfileid: "74225962"
---
# <a name="how-azure-databricks-pre-purchase-discount-is-applied"></a>Azure Databricks-korting bij aankoop vooraf toepassen

U kunt vooraf gekochte Azure Databricks-doorvoereenheden (DBCU's) gedurende de gehele aankooptermijn gebruiken. Elk gebruik van Azure Databricks wordt automatisch in mindering gebracht op de vooraf gekochte DBCU's.

In tegenstelling tot VM's verlopen vooraf gekochte eenheden niet per uur. U kunt ze op elk gewenst moment gebruiken tijdens de termijn van de aankoop. Om te beschikken over de korting bij aankoop vooraf is het niet nodig om een vooraf gekocht abonnement opnieuw te implementeren of toe te wijzen aan uw Azure Databricks-werkruimten voor het gebruik.

De korting bij aankoop vooraf geldt alleen voor het gebruik van Azure Databricks-eenheden (DBU). Andere kosten, zoals die voor berekeningen, opslag en netwerken, worden afzonderlijk in rekening gebracht.

## <a name="pre-purchase-discount-application"></a>Toepassing van korting bij aankoop vooraf

Databricks-korting bij aankoop vooraf is van toepassing op alle Databricks-workloads en -lagen. U kunt de aankoop vooraf beschouwen als een pool met vooraf betaalde Databricks-doorvoereenheden. Het gebruik wordt in mindering gebracht op de pool, ongeacht de workload of de laag. Hierbij wordt de volgende verhouding gehanteerd:

| **Workload** | **Verhouding DBU-toepassing - Standard-laag** | **Verhouding DBU-toepassing - Premium-laag** |
| --- | --- | --- |
| Data Analytics | 0,4 | 0,55 |
| Data Engineering | 0,15 | 0,30 |
| Data Engineering Light | 0,07 | 0,22 |

Als er bijvoorbeeld een hoeveelheid DBU's wordt gebruikt voor Data Analytics - Standard-laag, worden deze na vermenigvuldiging met 0,4 in mindering gebracht op de pool vooraf gekochte Databricks-doorvoereenheden. Als er een hoeveelheid DBU's wordt gebruikt voor Data Engineering Light - Standard-laag, worden deze na vermenigvuldiging met 0,07 in mindering gebracht op de pool vooraf gekochte Databricks-doorvoereenheden.

## <a name="determine-plan-use"></a>Het gebruik van het abonnement bepalen

Als u het gebruik van uw DBCU-abonnement wilt bepalen, gaat u naar de Azure-portal > **Reserveringen** en klikt u op het gekochte Databricks-abonnement. Uw gebruik tot heden wordt weer gegeven met eventuele alle resterende eenheden. Zie het artikel [Reserveringsgebruik](billing-reservation-apis.md#see-reservation-usage) voor meer informatie over het bepalen van het gebruik van reserveringen.

## <a name="how-discount-application-shows-in-usage-data"></a>Hoe kortingstoepassing wordt weergeven in gebruiksgegevens

Wanneer de korting bij aankoop vooraf van toepassing is op uw Databricks-gebruik, worden on-demand kosten als nul weergegeven in de gebruiksgegevens. Zie [Kosten en gebruik van Enterprise Agreement-reserveringen ophalen](billing-understand-reserved-instance-usage-ea.md) voor meer informatie over reserveringskosten en gebruik.

## <a name="need-help-contact-us"></a>Hebt u hulp nodig? Neem contact met ons op.

Als u vragen hebt of hulp nodig hebt, [kunt u een ondersteuningsaanvraag maken](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

## <a name="next-steps"></a>Volgende stappen

- Zie [Azure-reserveringen beheren](billing-manage-reserved-vm-instance.md) voor meer informatie over het beheren van een reservering.
- Zie [De kosten voor Azure Databricks optimaliseren door hiervoor vooraf te betalen](billing-prepay-databricks-reserved-capacity.md) voor meer informatie over het vooraf kopen van Azure Databricks om geld te besparen.
- Raadpleeg de volgende artikelen voor meer informatie over Azure-reserveringen:
  - [Wat zijn Azure-reserveringen?](billing-save-compute-costs-reservations.md)
  - [Reserveringen beheren in Azure](billing-manage-reserved-vm-instance.md)
  - [Inzicht in het gebruik van reserveringen voor uw abonnement met betalen per gebruik](billing-understand-reserved-instance-usage.md)
  - [Inzicht in het gebruik van reserveringen voor Enterprise-inschrijvingen](billing-understand-reserved-instance-usage-ea.md)
