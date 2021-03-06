---
title: Self-service voor wachtwoord herstel in Azure Active Directory B2C | Microsoft Docs
description: Laat zien hoe u selfservice voor wachtwoord herstel voor uw klanten instelt in Azure Active Directory B2C
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/30/2018
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 7903ec669772c3a3858a9c1d514ab3e6de6a2bd4
ms.sourcegitcommit: f2d9d5133ec616857fb5adfb223df01ff0c96d0a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/03/2019
ms.locfileid: "71936818"
---
# <a name="set-up-self-service-password-reset-for-your-customers"></a>Selfservice voor wacht woord opnieuw instellen instellen voor uw klanten

Met de self-service voor het opnieuw instellen van wacht woorden kunnen uw klanten die zich hebben aangemeld voor lokale accounts hun wacht woord opnieuw instellen. Hierdoor wordt de belasting van uw ondersteunings medewerkers aanzienlijk verminderd, met name als uw toepassing op een regel matige basis gebruikmaakt van miljoenen klanten. Op dit moment is het gebruik van een geverifieerd e-mail adres de enige ondersteunde herstel methode.

> [!NOTE]
> Dit artikel is van toepassing op selfservice voor wachtwoord herstel die wordt gebruikt in de context van de V1- **aanmeldings in** de gebruikers stroom, waarbij het **lokale account-aanmelden** wordt gebruikt als id-provider. Raadpleeg [dit artikel](active-directory-b2c-reference-policies.md)als u volledig aanpas bare gebruikers stromen voor wachtwoord herstel nodig hebt die vanuit uw app worden aangeroepen.
> 
> 

Voor uw directory is standaard niet de selfservice voor wachtwoord herstel ingeschakeld. Gebruik de volgende stappen om deze in te scha kelen:

1. Meld u aan bij de [Azure Portal](https://portal.azure.com/) als abonnements beheerder. Dit is hetzelfde werk-of school account of hetzelfde Microsoft-account dat u hebt gebruikt om uw map te maken.
2. Open **Azure Active Directory** (in de navigatie balk aan de linkerkant).
3. Schuif omlaag op de Blade opties en selecteer **wacht woord opnieuw instellen**.
4. Stel **selfservice voor wachtwoord herstel** in op **alle**. 
5. Klik bovenaan de pagina op **Opslaan**. U bent klaar.

Als u wilt testen, gebruikt u de functie nu uitvoeren op elke aanmeldings stroom van de gebruiker die lokale accounts heeft als een id-provider. Klik op de aanmeldings pagina voor het lokale account (waar u een e-mail adres en wacht woord of een gebruikers naam en wacht woord invoert) op **geen toegang tot uw account?** om de klant ervaring te controleren.

> [!NOTE]
> De self-service voor het opnieuw instellen van wacht woorden kan worden aangepast met behulp van de [bedrijfs huisstijl functie](../active-directory/fundamentals/customize-branding.md).
> 
> 

