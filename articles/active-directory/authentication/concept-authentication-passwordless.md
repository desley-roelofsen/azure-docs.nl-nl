---
title: Azure Active Directory aanmelding zonder wacht woord (preview-versie)
description: Meld u aan bij Azure AD met behulp van FIDO2-beveiligings sleutels of de Microsoft Authenticator-app (preview)
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 10/08/2019
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: librown
ms.collection: M365-identity-device-management
ms.openlocfilehash: 28d4dd3f0d4432930d62bb499fe72533b79d2a08
ms.sourcegitcommit: c38a1f55bed721aea4355a6d9289897a4ac769d2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/05/2019
ms.locfileid: "74848728"
---
# <a name="passwordless-authentication-options"></a>Verificatie opties met een wacht woord

Multi-factor Authentication (MFA) is een uitstekende manier om uw organisatie te beveiligen, maar gebruikers worden met de extra laag boven op de hoogte gehouden om hun wacht woord te onthouden. Verificatie methoden met een wacht woord zijn handiger omdat het wacht woord wordt verwijderd en vervangen door iets wat u hebt of iets dat u weet.

|   | Iets dat u hebt | Iets dat u weet of weet |
| --- | --- | --- |
| Zonder wachtwoord | Windows 10-apparaat, telefoon of beveiligings sleutel | Biometrische of pincode |

Elke organisatie heeft verschillende behoeften wanneer dit van toepassing is op verificatie. Micro soft biedt drie verificatie opties met een wacht woord:

- Windows Hello voor Bedrijven
- Microsoft Authenticator-app
- FIDO2-beveiligings sleutels

![Verificatie: beveiliging versus gebruiks gemak](./media/concept-authentication-passwordless/passwordless-convenience-security.png)

## <a name="windows-hello-for-business"></a>Windows Hello voor Bedrijven

Windows hello voor bedrijven is ideaal voor IT-mede werkers die hun eigen aangewezen Windows-PC hebben. De biometrische en pincode zijn rechtstreeks gebonden aan de PC van de gebruiker, waardoor de toegang van iemand anders dan de eigenaar wordt voor komen. Met PKI-integratie en ingebouwde ondersteuning voor eenmalige aanmelding (SSO) biedt Windows hello voor bedrijven een eenvoudige en handige methode voor probleemloze toegang tot bedrijfs bronnen on-premises en in de Cloud.

De [plannings handleiding](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-planning-guide) voor Windows hello voor bedrijven kan worden gebruikt om u te helpen bij het nemen van beslissingen over het type implementatie van Windows hello voor bedrijven en de opties die u moet overwegen.

## <a name="microsoft-authenticator-app"></a>App Microsoft Authenticator

Laat de telefoon van uw werk nemer een verificatie methode met een wacht woord maken. U kunt ook de Microsoft Authenticator-app gebruiken als een handige multi-factor Authentication-optie naast een wacht woord. Maar nu is het beschikbaar als optie met een wacht woord.

![Aanmelden bij micro soft Edge met de app Microsoft Authenticator](./media/concept-authentication-passwordless/concept-web-sign-in-microsoft-authenticator-app.png)

Hiermee wordt een wille keurige iOS-of Android-telefoon omgezet in een sterke, wacht woordloze referentie door gebruikers toe te staan zich aan te melden bij een platform of browser door een melding te ontvangen op de telefoon, een nummer te zoeken dat op het scherm wordt weer gegeven op de telefoon en vervolgens met behulp van de biometrische ( aanraken of gezicht) of pincode om te bevestigen.

## <a name="fido2-security-keys"></a>FIDO2-beveiligings sleutels

FIDO2-beveiligings sleutels zijn een niet-verwerkte op standaarden gebaseerde verificatie methode op basis van een wacht woord, die in elke vorm factor kan worden gebruikt. Fast Identity online (FIDO) is een open standaard voor verificatie met een wacht woord. Hiermee kunnen gebruikers en organisaties de standaard gebruiken om zich aan te melden bij hun resources zonder gebruikers naam of wacht woord met behulp van een externe beveiligings sleutel of een platform sleutel die is ingebouwd in een apparaat.

Werk nemers kunnen met de open bare preview beveiligings sleutels gebruiken om zich aan te melden bij hun Azure AD-apparaten die lid zijn van Windows 10 en eenmalige aanmelding op hun Cloud-en on-premises resources krijgen. Ze kunnen zich ook aanmelden bij ondersteunde browsers.

![Aanmelden bij micro soft Edge met een beveiligings sleutel](./media/concept-authentication-passwordless/concept-web-sign-in-security-key.png)

Hoewel er veel sleutels zijn die FIDO2 gecertificeerd door de FIDO-Alliantie, vereist micro soft een aantal optionele uitbrei dingen van de FIDO2-client-to-Authenticator-Protocol (CTAP) die door de leverancier moet worden geïmplementeerd om te zorgen voor maximale beveiliging en de beste reageren.

Een beveiligings sleutel **moet** de volgende functies en uitbrei dingen van het FIDO2 CTAP-protocol implementeren als compatibel met micro soft:

| # | Functie/uitbrei ding vertrouwen | Waarom is deze functie of extensie vereist? |
| --- | --- | --- |
| 1 | Residente sleutel | Met deze functie kan de beveiligings sleutel worden overdraagbaar, waar uw referentie wordt opgeslagen op de beveiligings sleutel. |
| 2 | Client pincode | Met deze functie kunt u uw referenties beveiligen met een tweede factor en van toepassing op beveiligings sleutels die geen gebruikers interface hebben. |
| 3 | hmac-secret | Deze uitbrei ding zorgt ervoor dat u zich kunt aanmelden bij uw apparaat wanneer het offline is of in de vliegtuig stand. |
| 4 | Meerdere accounts per RP | Deze functie zorgt ervoor dat u dezelfde beveiligings sleutel kunt gebruiken voor meerdere services, zoals micro soft-account en Azure Active Directory. |

De volgende providers bieden FIDO2-beveiligings sleutels van verschillende vorm factoren waarvan bekend is dat ze compatibel zijn met de ervaring zonder wacht woord. Micro soft raadt klanten aan om de beveiligings eigenschappen van deze sleutels te evalueren door contact op te nemen met de leverancier en FIDO Alliance.

| Provider | Contact |
| --- | --- |
| Yubico | [https://www.yubico.com/support/contact/](https://www.yubico.com/support/contact/) |
| Feitian | [https://www.ftsafe.com/about/Contact_Us](https://www.ftsafe.com/about/Contact_Us) |
| SPECIALE | [https://www.hidglobal.com/contact-us](https://www.hidglobal.com/contact-us) |
| Ensurity | [https://www.ensurity.com/contact](https://www.ensurity.com/contact) |
| eWBM | [https://www.ewbm.com/support](https://www.ewbm.com/support) |
| AuthenTrend | [https://authentrend.com/about-us/#pg-35-3](https://authentrend.com/about-us/#pg-35-3) |

> [!NOTE]
> Als u voor het kopen en plannen van NFC-beveiligings sleutels wilt gebruiken, hebt u een ondersteunde NFC-lezer nodig.

Als u een leverancier bent en uw apparaat in deze lijst wilt ontvangen, neemt u contact op met [Fido2Request@Microsoft.com](mailto:Fido2Request@Microsoft.com).

FIDO2-beveiligings sleutels zijn een uitstekende optie voor ondernemingen die zeer gevoelig zijn voor bedrijven of die scenario's of werk nemers hebben die hun telefoon niet kunnen gebruiken als een tweede factor.

## <a name="what-scenarios-work-with-the-preview"></a>Welke scenario's werken met de preview-versie?

- Beheerders kunnen verificatie methoden met een wacht woord toestaan voor hun Tenant
- Beheerders kunnen alle gebruikers bereiken of gebruikers/groepen selecteren binnen hun Tenant voor elke methode
- Eind gebruikers kunnen deze verificatie methoden zonder wacht woord registreren en beheren in hun account Portal
- Eind gebruikers kunnen zich aanmelden met deze verificatie methoden met een wacht woord
   - Microsoft Authenticator-app: werkt in scenario's waarin Azure AD-verificatie wordt gebruikt, inclusief alle browsers, tijdens de installatie van Windows 10 out of Box (OOBE), en met geïntegreerde mobiele apps op elk besturings systeem.
   - Beveiligings sleutels: werken op het vergrendelings scherm voor Windows 10 en het web in ondersteunde browsers, zoals micro soft Edge.

## <a name="next-steps"></a>Volgende stappen

[Opties voor passwordlesss voor FIDO2-beveiligings sleutel inschakelen in uw organisatie](howto-authentication-passwordless-security-key.md)

[Op telefoon gebaseerde opties voor wacht woord in uw organisatie inschakelen](howto-authentication-passwordless-phone.md)

### <a name="external-links"></a>Externe koppelingen

[FIDO Alliance](https://fidoalliance.org/)

[FIDO2 CTAP-specificatie](https://fidoalliance.org/specs/fido-v2.0-id-20180227/fido-client-to-authenticator-protocol-v2.0-id-20180227.html)
