---
title: Wat is er nieuw voor Azure AD in Microsoft 365 overheid? | Microsoft Docs
description: Meer informatie over enkele wijzigingen in Azure Active Directory (Azure AD) in het Cloud exemplaar van Microsoft 365 Government. Dit kan van invloed zijn op u.
services: active-directory
author: eross-msft
manager: daveba
ms.author: lizross
ms.reviewer: sumitp
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 05/07/2019
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 75e7c6809810a78c81348bd4bbeb808e103f453f
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/08/2019
ms.locfileid: "73805288"
---
# <a name="whats-new-for-azure-active-directory-in-microsoft-365-government"></a>Wat is er nieuw voor Azure Active Directory in Microsoft 365 overheid

Er zijn enkele wijzigingen aangebracht in Azure Active Directory (Azure AD) in het Cloud exemplaar van Microsoft 365 Government, dat van toepassing is op klanten die de volgende services gebruiken:

- Microsoft Azure en de overheid

- Microsoft 365 overheid – GCC hoog

- Microsoft 365 overheid – DoD

Dit artikel is niet van toepassing op Microsoft 365 Government-GCC-klanten.

## <a name="changes-to-the-initial-domain-name"></a>Wijzigingen in de oorspronkelijke domein naam

Tijdens de eerste aanmelding van uw organisatie voor een online-Microsoft 365 Government-service, hebt u gevraagd de domein naam van uw organisatie te kiezen `<your-domain-name>.onmicrosoft.com`. Als u al een domein naam met het achtervoegsel. com hebt, wordt niets gewijzigd.

Als u zich echter aanmeldt voor een nieuwe Microsoft 365 overheids service, wordt u gevraagd een domein naam te kiezen met behulp van het `.us` achtervoegsel. Dit wordt dus `<your-domain-name>.onmicrosoft.us`.

>[!Note]
>Deze wijziging is niet van toepassing op klanten die worden beheerd door Cloud serviceproviders (Csp's).

## <a name="changes-to-portal-access"></a>Wijzigingen in de toegang tot de portal

We hebben de portal-eind punten voor Microsoft Azure Government bijgewerkt, Microsoft 365 Government – GCC High en Microsoft 365 Government – DoD, zoals wordt weer gegeven in de [eindpunt toewijzings tabel](#endpoint-mapping).

Voorheen kunnen klanten zich aanmelden met behulp van de wereld wijde Azure-portals (portal.azure.com) en Office 365 (portal.office.com). Met deze update moeten klanten zich nu aanmelden met behulp van de specifieke Microsoft Azure Government, Microsoft 365 Government-GCC High en Microsoft 365 Government-DoD-portals.

## <a name="endpoint-mapping"></a>Eindpunt toewijzing

In de volgende tabel worden de eind punten voor alle klanten weer gegeven:

| Naam | Details van eind punt |
|------|------------------|
| Portals |Microsoft Azure Government: https://portal.azure.us<p>Microsoft 365 overheid – GCC hoog: https://portal.office365.us<p>Microsoft 365 overheid – DoD: https://portal.apps.mil |
| Eind punt van Azure Active Directory instantie | https://login.microsoftonline.us |
| Azure Active Directory Graph API | https://graph.windows.net |
| Microsoft Graph-API voor Microsoft 365 Government GCC High | https://graph.microsoft.us |
| Microsoft Graph-API voor Microsoft 365 Government-DoD | https://dod-graph.microsoft.us |
| Azure Government services-eind punten | Zie [Azure Government ontwikkelaars handleiding](https://docs.microsoft.com/azure/azure-government/documentation-government-developer-guide) voor meer informatie |
| GCC High-eind punten Microsoft 365 overheid | Zie voor meer informatie [Office 365 Amerikaanse overheid gcc High-eind punten](https://docs.microsoft.com/office365/enterprise/office-365-u-s-government-gcc-high-endpoints) |
| Microsoft 365 overheid-DoD | Zie [Office 365 Amerikaanse Government DoD-eind punten](https://docs.microsoft.com/office365/enterprise/office-365-u-s-government-dod-endpoints) voor meer informatie |

## <a name="next-steps"></a>Volgende stappen

Raadpleeg voor meer informatie de volgende artikelen:

- [Wat is Azure Government?](https://docs.microsoft.com/azure/azure-government/documentation-government-welcome)

- [Azure Government AAD Authority-eind punt update](https://devblogs.microsoft.com/azuregov/azure-government-aad-authority-endpoint-update/)

- [Microsoft Graph eind punten in de Amerikaanse overheids Cloud](https://developer.microsoft.com/graph/blogs/new-microsoft-graph-endpoints-in-us-government-cloud/)

- [Office 365 Amerikaanse overheid GCC hoog en DoD](https://docs.microsoft.com/office365/servicedescriptions/office-365-platform-service-description/office-365-us-government/gcc-high-and-dod)