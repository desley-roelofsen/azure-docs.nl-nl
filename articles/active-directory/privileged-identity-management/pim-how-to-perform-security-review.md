---
title: Toegang tot Azure AD-rollen controleren in PIM-Azure AD | Microsoft Docs
description: Meer informatie over het controleren van de toegang tot Azure Active Directory rollen in Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 10/22/2019
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 76eccb5d62b68865b7a117312be62753f203e2cb
ms.sourcegitcommit: 16c5374d7bcb086e417802b72d9383f8e65b24a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/08/2019
ms.locfileid: "73847094"
---
# <a name="review-access-to-azure-ad-roles-in-privileged-identity-management"></a>Toegang tot Azure AD-rollen in Privileged Identity Management controleren

Privileged Identity Management (PIM) vereenvoudigt de manier waarop bedrijven bevoorrechte toegang beheren tot bronnen in Azure Active Directory (AD) en andere micro soft onlineservices, zoals Office 365 of Microsoft Intune. Volg de stappen in dit artikel om uw toegewezen rollen zelf te controleren.

Als u bent toegewezen aan een administratieve rol, vraagt de beheerder van de bevoegde rol van uw organisatie u mogelijk regel matig te bevestigen dat u deze rol nog nodig hebt voor uw taak. U kunt ook een e-mail bericht met een koppeling ontvangen, maar direct naar de [Azure Portal](https://portal.azure.com) en beginnen.

Als u een beheerder van een bevoorrechte rol of globale beheerder bent die geïnteresseerd is in toegangs beoordelingen, kunt u meer informatie vinden [over het starten van een toegangs beoordeling](pim-how-to-start-security-review.md).

## <a name="add-a-pim-dashboard-tile"></a>Een PIM-dashboard tegel toevoegen

Als u de Azure AD Privileged Identity Management-service niet hebt vastgemaakt aan uw dash board in uw Azure Portal, voert u de volgende stappen uit om aan de slag te gaan.

1. Meld u aan bij de [Azure Portal](https://portal.azure.com/).
2. Selecteer uw gebruikers naam in de rechter bovenhoek van de Azure Portal en selecteer de map waarin u wilt werken.
3. Selecteer **Alle services** en gebruik het tekstvak Filteren om te zoeken naar **Azure AD Privileged Identity Management**.
4. Schakel **Vastmaken aan dashboard** in en klik op de knop **Maken**. De Privileged Identity Management-toepassing wordt geopend.

## <a name="approve-or-deny-access"></a>Toegang goed keuren of weigeren

Wanneer u toegang goedkeurt of weigert, vertelt u de revisor of u deze functie nog gebruikt of niet. Kies **goed keuren** als u de rol wilt blijven gebruiken of **weigeren** als u de toegang niet meer nodig hebt. De status wordt niet meteen gewijzigd, totdat de revisor de resultaten toepast.
Volg deze stappen om de toegangs beoordeling te vinden en te volt ooien:

1. Selecteer in de Privileged Identity Management-service **privileged Access controleren**. Als u openstaande toegangs beoordelingen hebt, worden deze weer gegeven op de pagina Azure AD- **toegangs beoordelingen** .
2. Selecteer de beoordeling die u wilt volt ooien.
3. Tenzij u de beoordeling hebt gemaakt, wordt u als de enige gebruiker in de beoordeling weer gegeven. Schakel het vinkje naast uw naam in.
4. Kies **goed keuren** of **weigeren**. Mogelijk moet u in het tekstvak **een reden opgeven** een reden voor uw beslissing op te nemen.  
5. Sluit de Blade **Azure AD-rollen controleren** .

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Volgende stappen

- [Perform an access review of my Azure AD resource roles in PIM](pim-resource-roles-perform-access-review.md) (Een toegangsbeoordeling uitvoeren van mijn Azure AD-resourcerollen in PIM)
