---
title: Azure Active Directory rapportage latenties | Microsoft Docs
description: Meer informatie over de hoeveelheid tijd die nodig is voor het rapporteren van gebeurtenissen om weer te geven in uw Azure Portal
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 9b88958d-94a2-4f4b-a18c-616f0617a24e
ms.service: active-directory
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 05/13/2019
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6d031546bb4f1f05e9ea2abb5b74fe911b0b507f
ms.sourcegitcommit: ae8b23ab3488a2bbbf4c7ad49e285352f2d67a68
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/13/2019
ms.locfileid: "74007704"
---
# <a name="azure-active-directory-reporting-latencies"></a>Vertragingen bij rapportage Azure Active Directory

Latentie is de hoeveelheid tijd die nodig is voor het weer geven van Azure Active Directory (Azure AD)-rapportage gegevens in de [Azure Portal](https://portal.azure.com). In dit artikel wordt de verwachte latentie voor de verschillende typen rapporten weer gegeven. 

## <a name="activity-reports"></a>Activiteitsrapporten

Er zijn twee soorten activiteiten rapporten:

- [Aanmeldingen](concept-sign-ins.md) : bevat informatie over het gebruik van beheerde toepassingen en aanmeldings activiteiten voor gebruikers
- [Audit logboeken](concept-audit-logs.md) : bevat informatie over de systeem activiteit over gebruikers en groepen, beheerde toepassingen en Directory-activiteiten

De volgende tabel bevat de latentie-informatie voor activiteiten rapporten. 

> [!NOTE]
> **Latentie (95e percentiel)** verwijst naar het tijdstip waarop 95% van de logboeken worden gerapporteerd en **latentie (99e percentiel)** verwijst naar het tijdstip waarop 99% van de logboeken worden gerapporteerd. 
>

| Rapport | Latentie (95e percentiel) |Latentie (99e percentiel)|
| :-- | --- | --- |
| Auditlogboeken | 2 minuten  | 5 minuten  |
| Aanmeldingen | 2 minuten  | 5 minuten |

### <a name="how-soon-can-i-see-activities-data-after-getting-a-premium-license"></a>Hoe kan ik activiteiten gegevens weer geven na het verkrijgen van een Premium-licentie?

Als u al activiteiten gegevens met uw gratis licentie hebt, kunt u deze direct na de upgrade bekijken. Als u geen gegevens hebt, duurt het één of twee dagen voordat de gegevens in de rapporten worden weer gegeven nadat u een upgrade hebt uitgevoerd naar een Premium-licentie.

## <a name="security-reports"></a>Beveiligingsrapporten

Er zijn twee typen beveiligingsrapporten:

- [Riskante aanmeldingen](concept-risky-sign-ins.md) - Een riskante aanmelding is een indicator van een aanmeldingspoging die mogelijk is uitgevoerd door iemand die geen rechtmatige eigenaar van een gebruikersaccount is. 
- [Gebruikers van wie wordt aangegeven dat ze risico lopen](concept-user-at-risk.md) - Een riskante gebruiker is een indicator van een gebruikersaccount dat mogelijk is aangetast. 

De volgende tabel bevat de latentie-informatie voor beveiligings rapporten.

| Rapport | Minimum | Average | Maximum |
| :-- | --- | --- | --- |
| Gebruikers die risico lopen          | 5 minuten   | 15 minuten  | 2 uur  |
| Riskante aanmeldingen         | 5 minuten   | 15 minuten  | 2 uur  |

## <a name="risk-detections"></a>Risicodetectie

Azure AD gebruikt adaptieve machine learning algoritmen en heuristiek om verdachte acties te detecteren die betrekking hebben op uw gebruikers accounts. Elke gedetecteerde verdachte actie wordt opgeslagen in een record met de naam **risico detectie**.

De volgende tabel bevat de latentie-informatie voor risico detectie.

| Rapport | Minimum | Average | Maximum |
| :-- | --- | --- | --- |
| Aanmeldingen vanaf anonieme IP-adressen |5 minuten |15 minuten |2 uur |
| Aanmeldingen vanaf onbekende locaties |5 minuten |15 minuten |2 uur |
| Gebruikers van wie de referenties zijn gelekt |2 uur |4 uur |8 uur |
| Onmogelijke reis naar ongewone locaties |5 minuten |1 uur |8 uur  |
| Aanmeldingen vanaf geïnfecteerde apparaten |2 uur |4 uur |8 uur  |
| Aanmeldingen van IP-adressen met verdachte activiteit |2 uur |4 uur |8 uur  |


## <a name="next-steps"></a>Volgende stappen

* [Overzicht van Azure AD-rapporten](overview-reports.md)
* [Programmatische toegang tot Azure AD-rapporten](concept-reporting-api.md)
* [Detectie van Azure Active Directory-Risico's](concept-risk-events.md)
