---
title: Probleem bij het configureren van federatieve eenmalige aanmelding voor een niet-galerie toepassing | Microsoft Docs
description: Adresseer een aantal veelvoorkomende problemen die kunnen optreden bij het configureren van federatieve eenmalige aanmelding voor uw aangepaste SAML-toepassing die niet wordt vermeld in de Azure AD-toepassings galerie
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: mimart
ms.collection: M365-identity-device-management
ROBOTS: NOINDEX
ms.openlocfilehash: 99c5e4d99f45e2a642a46f7dc070fb7512ff4d73
ms.sourcegitcommit: 198c3a585dd2d6f6809a1a25b9a732c0ad4a704f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/23/2019
ms.locfileid: "68422552"
---
# <a name="problem-configuring-federated-single-sign-on-for-a-non-gallery-application"></a>Probleem bij het configureren van federatieve eenmalige aanmelding voor een niet-galerie toepassing

Als er een probleem optreedt bij het configureren van een toepassing. Controleer of u alle stappen in het artikel voor het [configureren van eenmalige aanmelding voor toepassingen die zich niet in de Azure Active Directory toepassings galerie bevinden](https://docs.microsoft.com/azure/active-directory/application-config-sso-how-to-configure-federated-sso-non-gallery) hebt gevolgd.

## <a name="cant-add-another-instance-of-the-application"></a>Kan geen ander exemplaar van de toepassing toevoegen

Als u een tweede exemplaar van een toepassing wilt toevoegen, moet u het volgende kunnen doen:

-   Configureer een unieke id voor het tweede exemplaar. U kunt niet dezelfde id configureren die wordt gebruikt voor het eerste exemplaar.

-   Configureer een ander certificaat dan dat voor het eerste exemplaar.

Als de toepassing geen van de voor gaande instanties ondersteunt, kunt u een tweede exemplaar niet configureren.

## <a name="where-do-i-set-the-entityid-user-identifier-format"></a>Waar kan ik de EntityID-indeling (gebruikers-id) instellen

U kunt de EntityID-indeling (gebruikers-id) niet selecteren die door Azure AD wordt verzonden naar de toepassing in het antwoord na de verificatie van de gebruiker.

Azure AD selecteert de indeling voor het NameID-kenmerk (gebruikers-id) op basis van de geselecteerde waarde of de indeling die door de toepassing is aangevraagd in het SAML-AuthRequest. Ga voor meer informatie naar het artikel [SAML-protocol voor eenmalige aanmelding](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) in de sectie NameIDPolicy,

## <a name="where-do-i-get-the-application-metadata-or-certificate-from-azure-ad"></a>Waar vind ik de meta gegevens van de toepassing of het certificaat van Azure AD

Voer de volgende stappen uit om de meta gegevens of het certificaat van de toepassing te downloaden uit Azure AD:

1. Open de [**Azure Portal**](https://portal.azure.com/) en meld u aan als **globale beheerder** of **co-beheerder.**

2. Open de **uitbrei ding Azure Active Directory** door te klikken op **alle services** boven aan het hoofd navigatie menu aan de linkerkant.

3. Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4. Klik op **bedrijfs toepassingen** in het navigatie menu Azure Active Directory aan de linkerkant.

5. Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

   * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle Toepassingen.**

6. Selecteer de toepassing waarvoor u eenmalige aanmelding hebt geconfigureerd.

7. Zodra de toepassing is geladen, klikt u op de **eenmalige aanmelding** in het navigatie menu aan de linkerkant van de toepassing.

8. Ga naar de sectie **SAML-handtekening certificaat** en klik vervolgens op kolom waarde **downloaden** . Afhankelijk van wat de toepassing nodig heeft om eenmalige aanmelding te configureren, ziet u de optie voor het downloaden van de meta gegevens-XML of het certificaat.

Azure AD biedt geen URL om de meta gegevens op te halen. De meta gegevens kunnen alleen worden opgehaald als een XML-bestand.

## <a name="dont-know-how-to-customize-saml-claims-sent-to-an-application"></a>Weet u niet hoe u de SAML-claims kunt aanpassen die naar een toepassing worden verzonden

Zie [claim toewijzing in azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) voor meer informatie over het aanpassen van de SAML-kenmerk claims die naar uw toepassing worden verzonden.

## <a name="next-steps"></a>Volgende stappen
[Toepassingen beheren met Azure Active Directory](what-is-application-management.md)
