---
title: Gebruikers naam opzoeken tijdens aanmelden Azure Active Directory | Microsoft Docs
description: Hoe scherm berichten de gebruikers naam opzoeken weer gegeven tijdens het aanmelden in Azure Active Directory
services: active-directory
author: curtand
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 11/08/2019
ms.author: curtand
ms.reviewer: kexia
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: c8b6a65a964016f702fcf75aa4cbdab33a952e3b
ms.sourcegitcommit: 49cf9786d3134517727ff1e656c4d8531bbbd332
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/13/2019
ms.locfileid: "74024253"
---
# <a name="home-realm-discovery-for-azure-active-directory-sign-in-pages"></a>Home realm-detectie voor Azure Active Directory aanmeldings pagina's

We wijzigen het aanmeldingsgedrag voor Azure Active Directory (Azure AD) zodat we nieuwe verificatiemethoden kunnen gebruiken en de bruikbaarheid kunnen verbeteren. Tijdens de aanmelding bepaalt Azure AD of een gebruiker verificatie moet uitvoeren. Azure AD neemt intelligente beslissingen door de instellingen van de organisatie en van gebruikers af te lezen voor de gebruikersnaam die op de aanmeldingspagina is ingevoerd. Hiermee zijn we weer een stap verder wat een toekomst zonder wachtwoorden betreft en waarmee extra referenties zoals FIDO 2.0 mogelijk zijn.

## <a name="home-realm-discovery-behavior"></a>Gedrag van detectie van thuis realm

In het verleden is de detectie van de Home-realm onderhevig aan het domein dat wordt meegeleverd bij het aanmelden of door een beleid voor de introductie van realm-detectie voor sommige oudere toepassingen. Zo kan een Azure Active Directory gebruiker in ons detectie gedrag de gebruikers naam van een fout typen, maar wel op het scherm van de referentie verzameling van het bedrijf arriveren. Dit gebeurt wanneer de gebruiker de domein naam ' contoso.com ' van de organisatie correct levert. Deze methode biedt echter niet de nauwkeurigheid die nodig is om de ervaring voor afzonderlijke gebruikers aan te passen.

Als u een breder scala aan referenties wilt ondersteunen en de bruikbaarheid wilt verg Roten, wordt het gedrag van de gebruikers naam voor het zoeken van Azure Active Directory tijdens het aanmeldings proces nu bijgewerkt. Het nieuwe gedrag zorgt voor intelligente beslissingen door de instellingen voor tenants en gebruikers niveau te lezen op basis van de gebruikers naam die op de aanmeldings pagina is opgegeven. Om dit mogelijk te maken, controleert Azure Active Directory of de gebruikers naam die is ingevoerd op de aanmeldings pagina bestaat in het opgegeven domein of de gebruiker wordt omgeleid om zijn of haar referenties op te geven.

Een extra voor deel van dit werk is het verbeteren van de fout berichten. Hier volgen enkele voor beelden van de verbeterde fout berichten bij het aanmelden bij een toepassing die alleen Azure Active Directory gebruikers ondersteunt.

- De gebruikers naam is onjuist getypt of de gebruikers naam is nog niet gesynchroniseerd met Azure AD:
  
    ![de gebruikers naam is onjuist getypt of niet gevonden](./media/signin-realm-discovery/typo-username.png)
  
- De domein naam is niet goed getypt:
  
    ![de domein naam is onjuist getypt of niet gevonden](./media/signin-realm-discovery/typo-domain.png)
  
- De gebruiker probeert zich aan te melden met een bekend consument domein:
  
    ![aanmelden met een bekend consument domein](./media/signin-realm-discovery/consumer-domain.png)
  
- Het wacht woord is onjuist getypt, maar de gebruikers naam is nauw keurig:  
  
    ![het wacht woord is niet juist getypt met een goede gebruikers naam](./media/signin-realm-discovery/incorrect-password.png)
  
> [!IMPORTANT]
> Deze functie kan van invloed zijn op federatieve domeinen die afhankelijk zijn van de oude basis realm-detectie op domein niveau om de Federatie af te dwingen. Raadpleeg [Home realm Discovery tijdens het aanmelden voor Microsoft 365 Services](https://azure.microsoft.com/updates/signin-hrd/)voor updates over het toevoegen van federatieve domein ondersteuning. Ondertussen hebben sommige organisaties hun werk nemers getraind om zich aan te melden met een gebruikers naam die niet bestaat in Azure Active Directory, maar bevat de juiste domein naam, omdat de domein namen gebruikers naar het domein eindpunt van de organisatie routeren. Het nieuwe aanmeldings gedrag staat dit niet toe. De gebruiker wordt op de hoogte gesteld om de gebruikers naam te corrigeren en ze zijn niet gemachtigd om zich aan te melden met een gebruikers naam die niet voor komt in Azure Active Directory.
>
> Als u of uw organisatie procedures heeft die afhankelijk zijn van het oude gedrag, is het belang rijk dat beheerders zich aanmelden en verificatie documentatie bijwerken en werk nemers trainen om hun Azure Active Directory gebruikers naam te gebruiken om zich aan te melden.
  
Als u problemen hebt met het nieuwe gedrag, laat u uw opmerkingen in het gedeelte **feedback** van dit artikel staan.  

## <a name="next-steps"></a>Volgende stappen

[Uw huis stijl voor aanmelden aanpassen](../fundamentals/add-custom-domain.md)
