---
title: De naam of het logo van een bedrijfs toepassing in azure AD wijzigen
description: De naam of het logo wijzigen voor een aangepaste bedrijfs toepassing in Azure Active Directory
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/05/2019
ms.author: mimart
ms.reviewer: asteen
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: d3dfe0f8788275dd4403b6c9cad99a8eb09e479b
ms.sourcegitcommit: 653e9f61b24940561061bd65b2486e232e41ead4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/21/2019
ms.locfileid: "74274716"
---
# <a name="change-the-name-or-logo-of-an-enterprise-application-in-azure-active-directory"></a>De naam of het logo van een bedrijfs toepassing in Azure Active Directory wijzigen

Het is eenvoudig om de naam of het logo te wijzigen voor een aangepaste bedrijfs toepassing in Azure Active Directory (Azure AD). U moet de juiste machtigingen hebben om deze wijzigingen door te voeren. u moet de maker van de aangepaste toepassing zijn.

## <a name="how-do-i-change-an-enterprise-applications-name-or-logo"></a>Hoe kan ik de naam of het logo van een bedrijfs toepassing wijzigen?

1. Meld u aan bij de [Azure Active Directory-Portal](https://aad.portal.azure.com/) met een account dat een globale beheerder voor de Directory is. De pagina **Azure Active Directory beheer centrum** wordt weer gegeven.
2. Selecteer in het linkerdeelvenster **Enterprise-toepassingen**. De lijst met uw zakelijke toepassingen wordt weer gegeven.
3. Selecteer een toepassing. De overzichts pagina van de toepassing wordt weer gegeven.
4. Selecteer in het deel venster Overzicht van toepassing, onder de kop **beheren** , de optie **Eigenschappen**. De pagina **Eigenschappen** wordt weer gegeven.
5. Als u de naam wilt wijzigen, selecteert u het vak **naam** , typt u de nieuwe naam en drukt u op **Enter**.
6. Als u het logo wilt wijzigen, gaat u naar het veld **logo** en selecteert u het mappictogram naast het vak **Selecteer een bestand** , dat zich onder de huidige logo afbeelding van de toepassing bevindt.

   ![De opdracht Eigenschappen selecteren](./media/change-name-or-logo-portal/change-logo.png)

   Als u het logo niet wijzigt, gaat u naar stap 8.
7. Selecteer in de bestands kiezer het bestand dat u als het nieuwe logo wilt. De naam van het bestand wordt weer gegeven in het vak onder de huidige logo afbeelding.

   > [!NOTE]
   > Voor Azure moet de logo afbeelding een PNG-bestand zijn en worden er beperkingen toegepast op breedte, hoogte en bestands grootte.
8. Selecteer **Opslaan**. Als u een nieuw logo hebt gekozen, wordt de afbeelding van het veld **logo** gewijzigd in overeenstemming met het nieuwe logo bestand.

## <a name="next-steps"></a>Volgende stappen

* [Snelstartgids: de groepen en leden van uw organisatie in Azure Active Directory weer geven](../fundamentals/active-directory-groups-view-azure-portal.md)
* [Een gebruiker of groep toewijzen aan een bedrijfs-app](assign-user-or-group-access-portal.md)
* [Een gebruiker of groeps toewijzing verwijderen uit een bedrijfs-app](remove-user-or-group-access-portal.md)
* [Aanmeldingen van gebruikers voor een bedrijfs-app uitschakelen](disable-user-sign-in-portal.md)
