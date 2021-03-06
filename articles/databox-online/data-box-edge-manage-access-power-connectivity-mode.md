---
title: Azure Data Box Edge toegang tot het apparaat, de kracht en connectiviteits modus | Microsoft Docs
description: Hierin wordt beschreven hoe u de toegangs-, Power-en connectiviteits modus beheert voor het Azure Data Box Edge apparaat waarmee gegevens kunnen worden overgebracht naar Azure
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 06/24/2019
ms.author: alkohli
ms.openlocfilehash: 7ce4b9dda853e63e427757317abc2f7c878ba3a4
ms.sourcegitcommit: a6873b710ca07eb956d45596d4ec2c1d5dc57353
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/16/2019
ms.locfileid: "68253171"
---
# <a name="manage-access-power-and-connectivity-mode-for-your-azure-data-box-edge"></a>De toegang, de kracht en de connectiviteits modus voor uw Azure Data Box Edge beheren

In dit artikel wordt beschreven hoe u de modus toegang, kracht en connectiviteit beheert voor uw Azure Data Box Edge. Deze bewerkingen worden uitgevoerd via de lokale webgebruikersinterface of de Azure Portal.

In dit artikel leert u het volgende:

> [!div class="checklist"]
> * Apparaattoegang beheren
> * Connectiviteits modus beheren
> * Energie beheren


## <a name="manage-device-access"></a>Apparaattoegang beheren

De toegang tot uw Data Box Edge-apparaat wordt bepaald door het gebruik van een apparaatwachtwoord. U kunt het wacht woord wijzigen via de lokale webgebruikersinterface. U kunt het wacht woord van het apparaat ook opnieuw instellen in de Azure Portal.

### <a name="change-device-password"></a>Wachtwoord voor apparaat wijzigen

Volg deze stappen in de lokale gebruikers interface om het wacht woord van het apparaat te wijzigen.

1. Ga in de lokale web-UI naar **onderhoud > wachtwoord wijziging**.
2. Voer het huidige wacht woord en vervolgens het nieuwe wacht woord in. Het opgegeven wacht woord moet tussen 8 en 16 tekens lang zijn. Het wacht woord moet drie van de volgende tekens bevatten: hoofd letters, kleine letters, cijfers en speciale tekens. Bevestig het nieuwe wacht woord.

    ![Wacht woord wijzigen](media/data-box-edge-manage-access-power-connectivity-mode/change-password-1.png)

3. Selecteer **wacht woord wijzigen**.
 
### <a name="reset-device-password"></a>Wacht woord van apparaat opnieuw instellen

Voor de werk stroom opnieuw instellen is niet vereist dat de gebruiker het oude wacht woord intrekt en is nuttig wanneer het wacht woord verloren is gegaan. Deze werk stroom wordt uitgevoerd in de Azure Portal.

1. Ga in het Azure Portal naar **overzicht > het beheerders wachtwoord opnieuw**in te stellen.

    ![Wachtwoord opnieuw instellen](media/data-box-edge-manage-access-power-connectivity-mode/reset-password-1.png)


2. Voer het nieuwe wacht woord in en bevestig het. Het opgegeven wacht woord moet tussen 8 en 16 tekens lang zijn. Het wacht woord moet drie van de volgende tekens bevatten: hoofd letters, kleine letters, cijfers en speciale tekens. Selecteer **opnieuw instellen**.

    ![Wachtwoord opnieuw instellen](media/data-box-edge-manage-access-power-connectivity-mode/reset-password-2.png)

## <a name="manage-resource-access"></a>Toegang tot resources beheren

Als u uw Data Box Edge/Data Box Gateway-, IoT Hub-en Azure Storage-resource wilt maken, moet u machtigingen hebben als Inzender of hoger op het niveau van een resource groep. U moet ook de bijbehorende resource providers registreren. Voor bewerkingen waarbij de activerings sleutel en referenties betrokken zijn, zijn er ook machtigingen voor het Azure Active Directory Graph API vereist. Deze worden beschreven in de volgende secties.

### <a name="manage-microsoft-azure-active-directory-graph-api-permissions"></a>Microsoft Azure Active Directory Graph API-machtigingen beheren

Bij het genereren van de activerings sleutel voor het Data Box Edge apparaat of het uitvoeren van bewerkingen waarvoor referenties zijn vereist, hebt u machtigingen nodig voor het Azure Active Directory van Graph API. De bewerkingen die referenties nodig hebben, kunnen zijn:

-  Een share maken met een gekoppeld opslag account.
-  Het maken van een gebruiker die toegang heeft tot de shares op het apparaat.

U moet `User` toegang hebben tot Active Directory Tenant als u dit moet `Read all directory objects`kunnen doen. U kunt geen gast gebruiker zijn omdat ze geen machtigingen hebben voor `Read all directory objects`. Als u een gast bent, worden de bewerkingen, zoals het genereren van een activerings sleutel, het maken van een share op uw Data Box Edge apparaat, het maken van een gebruiker, de configuratie van de Edge Compute-functie, het opnieuw instellen van het wacht woord voor het apparaat niet uitgevoerd.

Zie [standaard toegang voor beheerders, gebruikers en gast gebruikers](https://docs.microsoft.com/previous-versions/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes#default-access-for-administrators-users-and-guest-users-)voor meer informatie over het verlenen van toegang aan gebruikers Azure Active Directory Graph API.

### <a name="register-resource-providers"></a>Registreren van resourceproviders

Als u een resource in azure wilt inrichten (in het Azure Resource Manager model), hebt u een resource provider nodig die het maken van die resource ondersteunt. Als u bijvoorbeeld een virtuele machine wilt inrichten, moet u een resource provider micro soft. Compute in het abonnement hebben.
 
Resourceproviders worden geregistreerd op het niveau van het abonnement. Nieuwe Azure-abonnementen worden standaard vooraf geregistreerd bij een lijst met veelgebruikte resourceproviders. De resource provider voor micro soft. DataBoxEdge is niet opgenomen in deze lijst.

U hoeft geen toegangs machtigingen voor het abonnements niveau te verlenen zodat gebruikers resources zoals ' micro soft. DataBoxEdge ' kunnen maken binnen hun resource groepen waarvoor ze eigendoms rechten hebben, op voor waarde dat de resource providers voor deze resources al registratie.

Voordat u probeert een resource te maken, moet u ervoor zorgen dat de resource provider is geregistreerd in het abonnement. Als de resource provider niet is geregistreerd, moet u ervoor zorgen dat de gebruiker die de nieuwe resource maakt, voldoende rechten heeft om de vereiste resource provider te registreren op het abonnements niveau. Als u dit nog niet hebt gedaan, ziet u de volgende fout:

*De naam \<van het abonnements abonnement > heeft geen machtigingen om de resource provider (s) te registreren: Microsoft.DataBoxEdge.*


Voer de volgende opdracht uit om een lijst met geregistreerde resource providers in het huidige abonnement op te halen:

```PowerShell
Get-AzResourceProvider -ListAvailable |where {$_.Registrationstate -eq "Registered"}
```

Voor data Box edge apparaat `Microsoft.DataBoxEdge` moet worden geregistreerd. Als u `Microsoft.DataBoxEdge`de registratie wilt registreren, moet u de volgende opdracht uitvoeren:

```PowerShell
Register-AzResourceProvider -ProviderNamespace Microsoft.DataBoxEdge
```

Zie [fouten voor de registratie van de resource provider oplossen](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-register-provider-errors)voor meer informatie over het registreren van een resource provider.

## <a name="manage-connectivity-mode"></a>Connectiviteits modus beheren

Naast de standaard modus volledig verbonden, kan het apparaat ook worden uitgevoerd in een gedeeltelijk verbonden modus of in een volledig niet-verbrekende weer gave. Elk van deze modi wordt hieronder beschreven:

- **Volledig verbonden** : dit is de normale standaard modus waarin het apparaat actief is. Het uploaden en downloaden van gegevens in de Cloud is in deze modus ingeschakeld. U kunt de Azure Portal of de lokale web-UI gebruiken om het apparaat te beheren.

- **Gedeeltelijk verbroken** : in deze modus kan het apparaat geen share gegevens uploaden of downloaden, maar kan ook worden beheerd via de Azure Portal.

    Deze modus wordt doorgaans gebruikt wanneer op een satelliet netwerk met een Data limiet het doel is om het gebruik van de netwerk bandbreedte te minimaliseren. Mini maal netwerk verbruik kan nog steeds optreden voor bewerkingen voor het controleren van apparaten.

- **Verbinding verbroken** : in deze modus is het apparaat volledig losgekoppeld van de Cloud en worden de Cloud-uploads en down loads uitgeschakeld. Het apparaat kan alleen worden beheerd via de lokale webgebruikersinterface.

    Deze modus wordt doorgaans gebruikt wanneer u uw apparaat offline wilt zetten.

Voer de volgende stappen uit om de modus apparaat te wijzigen:

1. Ga in de lokale web-UI van uw apparaat naar **configuratie > Cloud instellingen**.
2. Selecteer in de vervolg keuzelijst de modus waarin u het apparaat wilt bedienen. U kunt een **volledig verbonden**, **gedeeltelijk verbonden**en **volledig losgekoppelde**selectie selecteren. Als u het apparaat wilt uitvoeren in een gedeeltelijk niet-verbonden modus, schakelt u **Azure Portal-beheer**in.

    ![Connectiviteits modus](media/data-box-edge-manage-access-power-connectivity-mode/connectivity-mode.png)
 
## <a name="manage-power"></a>Energie beheren

U kunt het fysieke apparaat afsluiten of opnieuw opstarten met behulp van de lokale webgebruikersinterface. U kunt het beste de shares offline halen op de gegevens server en vervolgens op het apparaat. Deze actie minimaliseert de kans op beschadiging van gegevens.

1. Ga in de lokale web-UI naar **onderhoud > energie-instellingen**.
2. Selecteer **Afsluiten** of **opnieuw opstarten** , afhankelijk van wat u wilt doen.

    ![Energie-instellingen](media/data-box-edge-manage-access-power-connectivity-mode/shut-down-restart-1.png)

3. Als u om bevestiging wordt gevraagd, selecteert u **Ja** om door te gaan.

> [!NOTE]
> Als u het fysieke apparaat afsluit, moet u de aan/uit-knop op het apparaat pushen om deze in te scha kelen.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [beheren](data-box-edge-manage-shares.md)van shares.
