---
title: Machtigingen in Azure Security Center | Microsoft Docs
description: In dit artikel wordt uitgelegd hoe Azure Security Center toegangs beheer op basis van rollen gebruikt om machtigingen aan gebruikers toe te wijzen en de toegestane acties voor elke rol te identificeren.
services: security-center
cloud: na
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: ''
ms.service: security-center
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/28/2018
ms.author: memildin
ms.openlocfilehash: f52d518d2ed1dfb62eed72cf9c0b839a37b7f856
ms.sourcegitcommit: 8a717170b04df64bd1ddd521e899ac7749627350
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/23/2019
ms.locfileid: "71201654"
---
# <a name="permissions-in-azure-security-center"></a>Machtigingen in Azure Security Center

Het Azure Beveiligingscentrum gebruikt [op rollen gebaseerd toegangsbeheer (RBAC)](../role-based-access-control/role-assignments-portal.md), dat [ingebouwde rollen](../role-based-access-control/built-in-roles.md) biedt die kunnen worden toegewezen aan gebruikers, groepen en services in Azure.

Security Center beoordeelt de configuratie van uw resources om beveiligingsproblemen met zich mee en beveiligingsproblemen te identificeren. In Security Center ziet u alleen informatie met betrekking tot een bron wanneer u de rol van eigenaar, bijdrager of lezer voor het abonnement of de resourcegroep die deel uitmaakt van een resource om te worden toegewezen.

Naast deze rollen zijn er twee specifieke Security Center-rollen:

* **Beveiligings lezer**: Een gebruiker die deel uitmaakt van deze rol heeft weergave rechten voor Security Center. De gebruiker kan aanbevelingen, waarschuwingen, een beveiligings beleid en beveiligings status weer geven, maar kan geen wijzigingen aanbrengen.
* **Beveiligings beheerder**: Een gebruiker die deel uitmaakt van deze rol heeft dezelfde rechten als de beveiligings lezer en kan het beveiligings beleid ook bijwerken en waarschuwingen en aanbevelingen negeren.

> [!NOTE]
> De beveiligings rollen, beveiligings lezer en beveiligings beheerder hebben alleen toegang tot Security Center. De beveiligings rollen hebben geen toegang tot andere service gebieden van Azure, zoals Storage, Web & Mobile of Internet of Things.
>
>

## <a name="roles-and-allowed-actions"></a>Rollen en toegestane acties

De volgende tabel bevat rollen en toegestane acties in Security Center. Een X geeft aan dat de actie is toegestaan voor die rol.

| Role | Beveiligings beleid bewerken | Aanbevelingen voor beveiliging Toep assen op een resource | Waarschuwingen en aanbevelingen negeren | Waarschuwingen en aanbevelingen weer geven |
|:--- |:---:|:---:|:---:|:---:|
| Abonnements eigenaar | X | X | X | X |
| Mede werker van abonnement | -- | X | X | X |
| Eigenaar van resource groep | -- | X | -- | X |
| Inzender voor resource groep | -- | X | -- | X |
| Lezer | -- | -- | -- | X |
| Beveiligingsbeheerder | X | -- | X | X |
| Beveiligingslezer | -- | -- | -- | X |

> [!NOTE]
> We raden u aan de rol toe te wijzen die gebruikers minimaal nodig hebben om hun taken uit te voeren. Wijs bijvoorbeeld de rol Lezer toe aan gebruikers die alleen informatie over de beveiligings status van een resource hoeven te bekijken, maar geen actie ondernemen, zoals het Toep assen van aanbevelingen of het bewerken van beleid.
>
>

## <a name="next-steps"></a>Volgende stappen
In dit artikel wordt uitgelegd hoe Security Center RBAC gebruikt om machtigingen toe te wijzen aan gebruikers en de toegestane acties voor elke rol te identificeren. Nu u bekend bent met de roltoewijzingen die nodig zijn voor het bewaken van de beveiligings status van uw abonnement, het bewerken van beveiligings beleid en het Toep assen van aanbevelingen, leert u het volgende:

- [Beveiligings beleid instellen in Security Center](tutorial-security-policy.md)
- [Aanbevelingen voor beveiliging in Security Center beheren](security-center-recommendations.md)
- [De beveiligings status van uw Azure-resources controleren](security-center-monitoring.md)
- [Beveiligingswaarschuwingen beheren en erop reageren in Security Center](security-center-managing-and-responding-alerts.md)
- [Beveiligings oplossingen van partners bewaken](security-center-partner-solutions.md)
