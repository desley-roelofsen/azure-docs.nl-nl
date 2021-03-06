---
title: Enterprise State Roaming in Azure Active Directory inschakelen | Microsoft Docs
description: Veelgestelde vragen over Enterprise State Roaming instellingen in Windows-apparaten.
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: conceptual
ms.date: 06/28/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: na
ms.collection: M365-identity-device-management
ms.openlocfilehash: f0f4a260bb52fb10147f6d6b9e74aa5cd4fd0e1a
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68562155"
---
# <a name="enable-enterprise-state-roaming-in-azure-active-directory"></a>Enterprise state roaming inschakelen in Azure Active Directory
Enterprise State Roaming is beschikbaar voor alle organisaties met een licentie voor Azure AD Premium of Enterprise Mobility + Security (EMS). Zie de [product pagina van Azure AD](https://azure.microsoft.com/services/active-directory)voor meer informatie over het verkrijgen van een Azure AD-abonnement.

Als u Enterprise State Roaming inschakelt, krijgt uw organisatie automatisch een gratis, beperkt gebruik van licenties voor Azure Rights Management-beveiliging van Azure Information Protection. Dit gratis abonnement is beperkt tot het versleutelen en ontsleutelen van ondernemings instellingen en toepassings gegevens die zijn gesynchroniseerd door Enterprise State Roaming. U moet [een betaald abonnement](https://azure.microsoft.com/pricing/details/information-protection/) hebben om de volledige mogelijkheden van de Azure Rights Management-service te kunnen gebruiken.

## <a name="to-enable-enterprise-state-roaming"></a>Enterprise State Roaming inschakelen

1. Meld u aan bij het [beheer centrum van Azure AD](https://aad.portal.azure.com/).
1. Selecteer **Azure Active Directory** &gt; apparaten EnterpriseStateroaming&gt; .
1. Selecteer **gebruikers kunnen instellingen en app-gegevens synchroniseren op verschillende apparaten**. Zie [Apparaatinstellingen configureren](https://docs.microsoft.com/azure/active-directory/device-management-azure-portal)voor meer informatie.
  
   ![afbeelding van de apparaat-instelling met de naam gebruikers kan instellingen en app-gegevens op verschillende apparaten synchroniseren](./media/enterprise-state-roaming-enable/device-settings.png)
  
Voor een Windows 10-apparaat om de Enterprise State Roaming-service te kunnen gebruiken, moet het apparaat worden geverifieerd met een Azure AD-identiteit. Voor apparaten die lid zijn van Azure AD, is de identiteit van de primaire aanmelding van de gebruiker hun Azure AD-identiteit, zodat er geen aanvullende configuratie is vereist. Voor apparaten die on-premises Active Directory gebruiken, moet de IT-beheerder [hybrid Azure Active Directory joind-apparaten configureren](hybrid-azuread-join-manual-steps.md). 

## <a name="data-storage"></a>Gegevensopslag
Enterprise State Roaming gegevens worden gehost in een of meer [Azure-regio's](https://azure.microsoft.com/regions/) die het beste overeenkomen met de waarde voor land/regio die is ingesteld in het Azure Active Directory exemplaar. Enterprise State Roaming gegevens worden gepartitioneerd op basis van drie primaire geografische regio's: Noord-Amerika, EMEA en APAC. Enterprise State Roaming gegevens voor de Tenant bevinden zich lokaal in de geografische regio en worden niet gerepliceerd tussen regio's.  Bijvoorbeeld:

| Waarde van land/regio | heeft hun gegevens gehost in |
| -------------------- | ------------------------ |
| Een EMEA-land/-regio, zoals Frank rijk of Zambia | Een of meer Azure-regio's in Europa |
| Een land/regio in het noordoosten, zoals Verenigde Staten of Canada | Een of meer Azure-regio's in de VS |
| Een APAC land/regio, zoals Australië of Nieuw-Zeeland | Een of meer Azure-regio's in Azië |
| Regio's Zuid-Amerika en Antarctica | Een of meer Azure-regio's in de VS |

De waarde van het land/de regio is ingesteld als onderdeel van het proces voor het maken van de Azure AD-Directory en kan niet worden gewijzigd. Als u meer informatie nodig hebt over de locatie van uw gegevens opslag, kunt u een ticket indienen met [ondersteuning voor Azure](https://azure.microsoft.com/support/options/).

## <a name="view-per-user-device-sync-status"></a>Synchronisatie status van apparaat per gebruiker weer geven
Volg deze stappen om een synchronisatie status rapport per gebruiker-apparaat weer te geven.

1. Meld u aan bij het [beheer centrum van Azure AD](https://aad.portal.azure.com/).
1. Selecteer **Azure Active Directory** &gt; gebruikers alle&gt; **gebruikers**.
1. Selecteer de gebruiker en selecteer vervolgens **apparaten**.
1. Onder **weer geven**selecteert u **apparaten synchroniseren instellingen en app-gegevens** om de synchronisatie status weer te geven.
  
   ![afbeelding van de instelling van de synchronisatie gegevens van het apparaat](./media/enterprise-state-roaming-enable/sync-status.png)
  
1. Als er apparaten worden gesynchroniseerd voor deze gebruiker, ziet u de apparaten zoals hier wordt weer gegeven.
  
   ![afbeelding van kolom met synchronisatie gegevens van apparaat](./media/enterprise-state-roaming-enable/device-status-row.png)

## <a name="data-retention"></a>Bewaartijd van gegevens
Gegevens die zijn gesynchroniseerd met de micro soft-Cloud met Enterprise State Roaming worden bewaard totdat deze hand matig wordt verwijderd of totdat de betreffende gegevens verouderd zijn. 

### <a name="explicit-deletion"></a>Expliciet verwijderen
Expliciet verwijderen is wanneer een Azure-beheerder een gebruiker of een map verwijdert of op andere wijze expliciet de gegevens moet worden verwijderd.

* **Gebruikers verwijdering**: Wanneer een gebruiker in azure AD wordt verwijderd, worden de zwervende gegevens van het gebruikers account na 90 tot 180 dagen verwijderd. 
* **Verwijderde mappen**: Het verwijderen van een volledige map in azure AD is een directe bewerking. Alle instellingen gegevens die aan die map zijn gekoppeld, worden na 90 tot 180 dagen verwijderd. 
* **Bij het verwijderen van aanvragen**: Als de Azure AD-beheerder hand matig de gegevens of instellingen van een specifieke gebruiker wil verwijderen, kan de beheerder een ticket indienen met [ondersteuning voor Azure](https://azure.microsoft.com/support/). 

### <a name="stale-data-deletion"></a>Verouderde gegevens verwijderen
Gegevens die gedurende één jaar niet zijn geopend ("de Bewaar periode"), worden behandeld als verouderd en kunnen worden verwijderd uit de micro soft-Cloud. De Bewaar periode is onderhevig aan wijzigingen, maar deze zijn niet minder dan 90 dagen. De verouderde gegevens kunnen een specifieke set instellingen voor Windows/Application of alle instellingen voor een gebruiker zijn. Bijvoorbeeld:

* Als er geen apparaten zijn die toegang hebben tot een bepaalde instellingen verzameling (bijvoorbeeld een toepassing wordt verwijderd van het apparaat of een instellingen groep zoals "thema" is uitgeschakeld voor alle apparaten van een gebruiker), wordt die verzameling verouderd na de Bewaar periode en kan deze worden verwijderd . 
* Als een gebruiker instellingen synchronisatie op al hun apparaten heeft uitgeschakeld, wordt geen van de instellingen gegevens geopend en worden alle instellingen voor die gebruiker verouderd en kunnen ze na de Bewaar periode worden verwijderd. 
* Als de Azure AD-beheerder Enterprise State Roaming uitschakelt voor de hele map, worden alle gebruikers in die map niet meer gesynchroniseerd met instellingen en worden alle instellingen voor alle gebruikers verouderd en kunnen ze na de Bewaar periode worden verwijderd. 

### <a name="deleted-data-recovery"></a>Herstel van gegevens verwijderd
Het Bewaar beleid voor gegevens kan niet worden geconfigureerd. Zodra de gegevens definitief zijn verwijderd, kan deze niet worden hersteld. De instellingen gegevens worden echter alleen verwijderd uit de micro soft-Cloud, niet vanaf het apparaat van de eind gebruiker. Als een apparaat later opnieuw verbinding maakt met de Enterprise State Roaming-service, worden de instellingen opnieuw gesynchroniseerd en opgeslagen in de micro soft-Cloud.

## <a name="next-steps"></a>Volgende stappen

* [Overzicht van Enterprise State Roaming](enterprise-state-roaming-overview.md)
* [Veelgestelde vragen over instellingen en gegevens roaming](enterprise-state-roaming-faqs.md)
* [groepsbeleid-en MDM-instellingen voor instellingen synchroniseren](enterprise-state-roaming-group-policy-settings.md)
* [Naslag informatie voor Windows 10-instellingen voor roaming](enterprise-state-roaming-windows-settings-reference.md)
* [Problemen oplossen](enterprise-state-roaming-troubleshooting.md)
