---
title: Problemen met dynamische groepslid maatschappen oplossen-Azure AD | Microsoft Docs
description: Tips voor het oplossen van problemen met een dynamisch groepslid maatschap in Azure Active Directory
services: active-directory
author: curtand
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 11/08/2019
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6f685ac63e3b4a8cf466be4eb4561472fb084d49
ms.sourcegitcommit: 49cf9786d3134517727ff1e656c4d8531bbbd332
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/13/2019
ms.locfileid: "74026549"
---
# <a name="troubleshoot-and-resolve-groups-issues"></a>Problemen met groepen oplossen en oplossen

## <a name="troubleshooting-group-creation-issues"></a>Problemen met het maken van groepen oplossen

**Ik heb het maken van een beveiligings groep uitgeschakeld in de Azure Portal, maar groepen kunnen nog steeds worden gemaakt via Power shell** De **gebruiker kan beveiligings groepen maken in** de instelling voor Azure-portals in de Azure Portal bepaalt of niet-beheerders gebruikers beveiligings groepen kunnen maken in het toegangs venster of het Azure Portal. Hiermee wordt het maken van een beveiligings groep niet beheerd via Power shell.

Het maken van groepen uitschakelen voor gebruikers die geen beheerder zijn in Power shell:
1. Controleer of niet-beheerders gebruikers groepen mogen maken:
   

   ```powershell
   Get-MsolCompanyInformation | Format-List UsersPermissionToCreateGroupsEnabled
   ```

  
2. Als deze `UsersPermissionToCreateGroupsEnabled : True`retourneert, kunnen gebruikers die geen beheerder zijn, groepen maken. U kunt deze functie als volgt uitschakelen:
  

   ``` 
   Set-MsolCompanySettings -UsersPermissionToCreateGroupsEnabled $False
   ```

<br/>**Ik heb een fout van het maximum aantal toegestane groepen ontvangen bij het maken van een dynamische groep in Power shell**<br/>
Als u een bericht ontvangt in Power shell dat _dynamisch groeps beleid aangeeft maximum aantal toegestane groepen is bereikt_, betekent dit dat u de maximum limiet hebt bereikt voor dynamische groepen in uw Tenant. Het maximum aantal dynamische groepen per Tenant is 5.000.

Als u nieuwe dynamische groepen wilt maken, moet u eerst enkele bestaande dynamische groepen verwijderen. Er is geen manier om de limiet te verhogen.

## <a name="troubleshooting-dynamic-memberships-for-groups"></a>Oplossen van problemen met dynamische lidmaatschappen voor groepen

**Ik heb een regel voor een groep geconfigureerd, maar er worden geen lidmaatschappen bijgewerkt in de groep**<br/>
1. Controleer de waarden voor gebruikers-of apparaateigenschappen in de regel. Zorg ervoor dat er gebruikers zijn die voldoen aan de regel. Voor apparaten controleert u de apparaateigenschappen om ervoor te zorgen dat gesynchroniseerde kenmerken de verwachte waarden bevatten.<br/>
2. Controleer de verwerkings status van de lidmaatschap om te controleren of deze is voltooid. U kunt de [verwerkings status](groups-create-rule.md#check-processing-status-for-a-rule) van het lidmaatschap en de datum waarop de update voor het laatst is bijgewerkt, op de pagina **overzicht** voor de groep controleren.

Als alles goed lijkt, wacht u even totdat de groep is gevuld. Afhankelijk van de grootte van uw tenant, kan het de eerste keer of na een regelwijziging tot 24 uur duren voordat de groep is ingevuld.

**Ik heb een regel geconfigureerd, maar nu worden de bestaande leden van de regel verwijderd**<br/>Dit is normaal gedrag. Bestaande leden van de groep worden verwijderd wanneer een regel wordt ingeschakeld of gewijzigd. De gebruikers die zijn geretourneerd door de evaluatie van de regel, worden toegevoegd als leden aan de groep.

**Ik zie lidmaatschaps wijzigingen niet direct wanneer ik een regel toevoeg of wijzig, waarom niet?**<br/>De toegewijde lidmaatschaps evaluatie wordt regel matig uitgevoerd in een asynchroon achtergrond proces. Hoe lang het proces duurt, wordt bepaald door het aantal gebruikers in uw map en de grootte van de groep die is gemaakt als gevolg van de regel. Meestal zien mappen met een klein aantal gebruikers dat het groepslid maatschap in minder dan een paar minuten wordt gewijzigd. Mappen met een groot aantal gebruikers kunnen 30 minuten of langer duren om in te vullen.

**Hoe kan ik afdwingen dat de groep nu wordt verwerkt?**<br/>
Op dit moment is er geen manier om de groep automatisch te activeren om op aanvraag te worden verwerkt. U kunt de herverwerking echter hand matig activeren door de lidmaatschaps regel bij te werken om een witruimte aan het einde toe te voegen.  

**Er is een fout opgetreden bij de verwerking van een regel**<br/>De volgende tabel bevat een lijst met veelvoorkomende fouten met dynamische lidmaatschaps regels en hoe u deze kunt corrigeren.

| Fout met regel-parser | Fout gebruik | Gecorrigeerd gebruik |
| --- | --- | --- |
| Fout: het kenmerk wordt niet ondersteund. |(User. invalidProperty-EQ "waarde") |(User. Department-EQ "waarde")<br/><br/>Zorg ervoor dat het kenmerk voor komt in de [lijst ondersteunde eigenschappen](groups-dynamic-membership.md#supported-properties). |
| Fout: de operator wordt niet ondersteund voor het kenmerk. |(User. accountEnabled-bevat True) |(User. accountEnabled-EQ True)<br/><br/>De operator die wordt gebruikt, wordt niet ondersteund voor het eigenschaps type (in dit voor beeld kan-contains niet worden gebruikt voor het type Boolean). Gebruik de juiste Opera tors voor het eigenschaps type. |
| Fout: compilatie fout van de query. | 1. (User. Department-EQ "Sales") (gebruiker. Department-EQ "marketing")<br>2.  (user.userPrincipalName -match "*@domain.ext") | 1. de operator ontbreekt. Gebruik-en of-of twee samenvoegings predikaten<br>(User. Department-EQ "Sales") of (User. Department-EQ "marketing")<br>2. fout in reguliere expressie die wordt gebruikt met-match<br>(user.userPrincipalName -match ".*@domain.ext")<br>of op een andere manier: (User. userPrincipalName-match "@domain.ext$") |

## <a name="next-steps"></a>Volgende stappen

Deze artikelen bevatten aanvullende informatie over Azure Active Directory.

* [Managing access to resources with Azure Active Directory groups](../fundamentals/active-directory-manage-groups.md) (Toegang tot resources beheren met Azure Active Directory-groepen)
* [Application Management in Azure Active Directory](../manage-apps/what-is-application-management.md) (Toepassingsbeheer in Azure Active Directory)
* [Wat is Azure Active Directory?](../fundamentals/active-directory-whatis.md)
* [Integrating your on-premises identities with Azure Active Directory (Engelstalig)](../hybrid/whatis-hybrid-identity.md)