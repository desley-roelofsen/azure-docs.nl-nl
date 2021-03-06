---
title: Naslag informatie voor app-registratie Portal | Azure
titleSuffix: Microsoft identity platform
description: Een beschrijving van de functies in de micro soft app registratie-Portal.
services: active-directory
author: rwike77
manager: CelesteDG
ms.assetid: f0507c28-9464-4d3e-bd53-de9053fd5278
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: conceptual
ms.date: 08/13/2019
ms.author: ryanwi
ms.reviewer: lenalepa
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1699aad41d8a5da153199f79f65af67bd04c6b2c
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/10/2019
ms.locfileid: "74966092"
---
# <a name="app-registration-reference"></a>Naslaginformatie over app-registratie

In dit document vindt u context en beschrijvingen van diverse functies die in de Azure Portal [app-registraties](https://aka.ms/appregistrations) -ervaring worden aangetroffen.

## <a name="my-applications-or-converged-applications"></a>Mijn toepassingen of geconvergeerde toepassingen

Deze lijst bevat alle toepassingen die zijn geregistreerd voor gebruik met het micro soft Identity platform (v 2.0)-eind punt. Met deze toepassingen kunnen gebruikers zich aanmelden met persoonlijke micro soft-accounts en werk-of school accounts van Azure Active Directory. Zie het [overzicht van v 2.0](active-directory-appmodel-v2-overview.md)voor meer informatie over het identiteits platform-eind punt. Deze toepassingen kunnen ook worden gebruikt om te integreren met het Microsoft-account verificatie-eind punt `https://login.live.com`.

## <a name="azure-ad-only-applications"></a>Alleen Azure AD-toepassingen

Deze lijst bevat alle toepassingen die zijn geregistreerd voor gebruik met het Azure AD v 1.0-eind punt. Met deze toepassingen kunnen gebruikers zich niet aanmelden met werk-of school accounts van Azure Active Directory. Deze lijst bevat toepassingen die zijn geregistreerd met behulp van de **app-registraties** -ervaring in de [Azure Portal](https://portal.azure.com).

## <a name="live-sdk-applications"></a>Live SDK-toepassingen

Deze lijst bevat alle toepassingen die zijn geregistreerd voor gebruik met Microsoft-account. Deze zijn niet ingeschakeld voor gebruik met Azure Active Directory. Hier vindt u alle toepassingen die eerder zijn geregistreerd bij de MSA-ontwikkelaars Portal op `https://account.live.com/developers/applications`. Alle functies die u eerder hebt uitgevoerd op `https://account.live.com/developers/applications`, kunnen nu worden uitgevoerd in [app-registraties](https://aka.ms/appregistrations).

## <a name="application-secrets"></a>Toepassingsgeheimen

Toepassings geheimen zijn referenties waarmee uw toepassing betrouw bare [client verificatie](https://tools.ietf.org/html/rfc6749#section-2.3) kan uitvoeren met Azure AD. In OAuth & OpenID Connect Connect wordt een toepassings geheim meestal een `client_secret`genoemd. In het v 2.0-protocol moet elke toepassing die een beveiligings token ontvangt op een webadresseer bare locatie (met behulp van een `https` schema) een toepassings geheim gebruiken om zichzelf te identificeren bij Azure AD na de afwisseling van dat beveiligings token. Daarnaast mag elke native client die tokens ontvangt op een apparaat, geen toepassings geheim gebruiken om client verificatie uit te voeren. Hiermee wordt de opslag van geheimen in onbeveiligde omgevingen geraden.

Elke app kan op elk gewenst moment twee geldige toepassings geheimen bevatten. Door twee geheimen te bewaren, hebt u de mogelijkheid om periodieke sleutel rollover uit te voeren in de gehele omgeving van uw toepassing. Wanneer u de volledige versie van uw toepassing naar een nieuw geheim hebt gemigreerd, kunt u het oude geheim verwijderen en een nieuw item inrichten.

Op dit moment zijn er slechts twee typen toepassings geheimen toegestaan in de app-registratie Portal. Als u **Nieuw wacht woord genereren** kiest, wordt een gedeeld geheim gegenereerd en opgeslagen in het respectieve gegevens archief, dat u in uw toepassing kunt gebruiken. Als u **Nieuw sleutel paar genereren** kiest, wordt er een nieuw openbaar/persoonlijk sleutel paar gemaakt dat kan worden gedownload en gebruikt voor client verificatie voor Azure AD. Als u de **open bare sleutel uploadt** , kunt u uw eigen open bare/persoonlijke sleutel paar gebruiken.
U moet een certificaat uploaden dat een open bare sleutel bevat.

## <a name="profile"></a>Profiel

De profiel sectie van de portal voor app-registratie kan worden gebruikt om de aanmeldings pagina voor uw toepassing aan te passen. Op dit moment kunt u het toepassings logo van de aanmeldings pagina, de voor waarden van de service-URL en de URL van de Privacyverklaring wijzigen. Het logo moet een transparante afbeelding van 48 x 48 of 50 x 50 pixels zijn in een GIF-, PNG-of JPEG-bestand met een grootte van 15 KB of kleiner. Wijzig de waarden en Bekijk de resulterende aanmeldings pagina.

## <a name="live-sdk-support"></a>Ondersteuning voor Live SDK

Wanneer u ' Live SDK-ondersteuning ' inschakelt, worden de door u gemaakte toepassings geheimen ingericht in de gegevens archieven van Azure AD en micro soft-account. Hierdoor kan uw toepassing rechtstreeks worden geïntegreerd met de micro soft-account service (login.live.com). Als u een app rechtstreeks wilt bouwen met behulp van micro soft-account (in plaats van het eind punt van de v 2.0), moet u ervoor zorgen dat de ondersteuning van Live SDK is ingeschakeld.

Als u live SDK-ondersteuning uitschakelt, wordt het toepassings geheim alleen geschreven in het Azure AD-gegevens archief. Het Azure AD-gegevens archief bevat bedrijfs kwaliteiten die aan bepaalde normen voldoen, zoals FISMA-naleving. Als u ondersteuning voor Live SDK inschakelt, kan het zijn dat uw toepassing niet voldoet aan enkele van deze standaarden.

Als u alleen van plan bent het v 2.0-eind punt te gebruiken, kunt u live SDK-ondersteuning veilig uitschakelen.
