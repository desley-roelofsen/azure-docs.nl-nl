---
title: Multi tenant-app toevoegen aan de Azure AD-toepassings galerie | Microsoft Docs
description: In dit artikel wordt uitgelegd hoe u uw aangepaste multi tenant-toepassing kunt weer geven in de Azure AD-toepassings galerie.
services: active-directory
documentationCenter: na
author: rwike77
manager: CelesteDG
ms.assetid: 92c1651a-675d-42c8-b337-f78e7dbcc40d
ms.service: active-directory
ms.subservice: develop
ms.custom: aaddev
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/11/2018
ms.author: ryanwi
ms.reviewer: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 65572b53591e3b94c26df988c61f8a79d15439cf
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/10/2019
ms.locfileid: "74962998"
---
# <a name="add-a-multitenant-application-to-the-azure-ad-application-gallery"></a>Een multi tenant-toepassing toevoegen aan de Azure AD-toepassings galerie

## <a name="what-is-the-azure-ad-application-gallery"></a>Wat is de Azure AD-toepassings galerie?

Azure Active Directory (Azure AD) is een op de cloud gebaseerde identiteits service. De [Azure AD-toepassings galerie](https://azure.microsoft.com/marketplace/active-directory/all/) bevindt zich in de Azure Marketplace-App Store, waar alle connectors voor toepassingen worden gepubliceerd voor eenmalige aanmelding en gebruikers inrichting. Klanten die Azure AD als id-provider gebruiken, vinden de verschillende SaaS Application connectors die hier zijn gepubliceerd. IT-beheerders kunnen Connect oren toevoegen vanuit de app-galerie en vervolgens de connectors configureren en gebruiken voor eenmalige aanmelding en inrichting. Azure AD biedt ondersteuning voor alle belang rijke Federatie protocollen, waaronder SAML 2,0, OpenID Connect Connect, OAuth en WS-insluiting voor eenmalige aanmelding. 

## <a name="if-your-application-supports-saml-or-openidconnect"></a>Als uw toepassing SAML of OpenIDConnect ondersteunt
Als u een multi tenant-toepassing hebt die u wilt weer geven in de Azure AD-toepassings galerie, moet u er eerst voor zorgen dat uw toepassing ondersteuning biedt voor een van de volgende technologieën voor eenmalige aanmelding:

- **OpenID Connect Connect**: als u uw app wilt weer geven, maakt u de multi tenant-toepassing in azure AD en implementeert u het [Azure AD-instemming raamwerk](https://docs.microsoft.com/azure/active-directory/develop/consent-framework) voor uw toepassing. Verzend de aanmeldings aanvraag naar een gemeen schappelijk eind punt zodat elke klant toestemming kan geven voor de toepassing. U kunt de toegang van een gebruiker beheren op basis van de Tenant-ID en de UPN van de gebruiker die in het token is ontvangen. Verzend de toepassing met behulp van het proces dat wordt beschreven in [uw toepassing weer geven in de galerie met Azure Active Directory toepassingen](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing).

- **SAML**: als uw toepassing SAML 2,0 ondersteunt, kan de app worden weer gegeven in de galerie. Volg de instructies in [uw toepassing weer geven in de galerie met Azure Active Directory toepassingen](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing).

## <a name="if-your-application-does-not-support-saml-or-openidconnect"></a>Als uw toepassing geen ondersteuning biedt voor SAML of OpenIDConnect
Toepassingen die geen ondersteuning bieden voor SAML of OpenIDConnect, kunnen nog steeds worden geïntegreerd in de app-galerie door middel van eenmalige aanmelding met een wacht woord.

Met eenmalige aanmelding met een wacht woord, ook wel wachtwoord kluizen genoemd, kunt u gebruikers toegang en-wacht woorden beheren voor webtoepassingen die geen identiteits Federatie ondersteunen. Het is ook handig voor scenario's waarin meerdere gebruikers één account moeten delen, zoals de accounts voor de sociale media-app van uw organisatie. 

Als u uw toepassing met deze technologie wilt vermelden:
1. Maak een webtoepassing met een HTML-aanmeldings pagina voor het configureren [van eenmalige aanmelding met een wacht woord](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis). 
2. Dien de aanvraag in, zoals wordt beschreven in [uw toepassing weer geven in de galerie met Azure Active Directory toepassingen](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing).

## <a name="escalations"></a>Escalaties

Voor eventuele escalaties verzendt u een e-mail naar het [Azure AD SSO-integratie team](<mailto:SaaSApplicationIntegrations@service.microsoft.com>) . we gaan zo snel mogelijk contact met u op.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over het [weer geven van uw toepassing in de galerie met Azure Active Directory toepassingen](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing).
