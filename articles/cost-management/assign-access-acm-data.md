---
title: Toegang tot Azure Cost Management gegevens toewijzen
description: Dit artikel helpt u al machtiging toewijzen aan Azure Cost Management-gegevens voor verschillende toegangsbereiken.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 10/14/2019
ms.topic: conceptual
ms.service: cost-management-billing
manager: vitavor
ms.custom: secdec18
ms.openlocfilehash: e3140ee990127db6815828314103a09dff7cf26e
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/20/2019
ms.locfileid: "74219852"
---
# <a name="assign-access-to-cost-management-data"></a>Toegang tot gegevens van kostenbeheer toewijzen

Voor gebruikers met Azure Enter prise Agreements, een combi natie van machtigingen die in de Azure Portal en de Enter prise-Portal worden verleend, wordt het toegangs niveau van een gebruiker voor Azure Cost Management gegevens gedefinieerd. Voor gebruikers met andere typen Azure-accounts kunt u het Cost Management toegangs niveau van een gebruiker met behulp van Azure op rollen gebaseerd toegangs beheer eenvoudiger definiëren. Dit artikel begeleidt u bij het toewijzen van toegang tot gegevens van Cost Management. Nadat de combinatie van machtigingen is toegewezen, is de gegevens van de weergaven in Cost Management op basis van het bereik dat ze toegang tot en op het bereik hebben dat ze in Azure portal selecteert.

Het adresbereik op dat een gebruiker selecteert wordt gebruikt in de gehele kostenbeheer voor gegevensconsolidatie en voor het beheren van toegang tot gegevens over de kosten. Bij het gebruik van bereiken, gebruikers geen meervoudige selectie ze. Ze selecteren in plaats daarvan een groter bereik die onderliggende bereiken tot draaien en vervolgens deze filter omlaag wat ze zoeken om weer te geven. Gegevensconsolidatie is belangrijk om te begrijpen omdat sommige mensen toegang tot een bovenliggend bereik die onderliggende bereiken tot mag niet hebt.

Bekijk [hoe u toegang kunt toewijzen met Azure Cost Management](https://www.youtube.com/watch?v=J997ckmwTa8) video voor meer informatie over het toewijzen van toegang voor het weer geven van kosten en kosten met op rollen gebaseerd toegangs beheer van Azure.

>[!VIDEO https://www.youtube.com/embed/J997ckmwTa8]

## <a name="cost-management-scopes"></a>Cost Management bereiken

Cost Management ondersteunt diverse typen Azure-accounts. Zie voor de volledige lijst met ondersteunde accounttypen [Gegevens van Azure Cost Management begrijpen](understand-cost-mgt-data.md). Het type account bepaalt de beschik bare bereiken.

### <a name="azure-ea-subscription-scopes"></a>Azure EA-abonnements bereiken

Voor het weer geven van kosten gegevens voor Azure EA-abonnementen moet een gebruiker ten minste lees toegang hebben tot een of meer van de volgende bereiken.

| **Bereik** | **Gedefinieerd op** | **Vereiste toegang om gegevens weer te geven** | **Vereiste EA-instelling** | **Consolideert gegevens tot** |
| --- | --- | --- | --- | --- |
| Factureringsaccount<sup>1</sup> | [https://ea.azure.com](https://ea.azure.com/) | Enterprise-beheerder | None | Alle abonnementen uit de Enterprise Overeenkomst |
| Afdeling | [https://ea.azure.com](https://ea.azure.com/) | Afdelingsbeheerder | **Da-weer gave-kosten** ingeschakeld | Alle abonnementen die horen bij een inschrijvingsaccount dat is gekoppeld aan de afdeling |
| Inschrijvingsaccount<sup>2</sup> | [https://ea.azure.com](https://ea.azure.com/) | Accounteigenaar | **Ao-weer gave-kosten** ingeschakeld | Alle abonnementen van het inschrijvingsaccount |
| Beheergroep | [https://portal.azure.com](https://portal.azure.com/) | Cost Management Reader (of Reader) | **Ao-weer gave-kosten** ingeschakeld | Alle abonnementen onder de beheergroep |
| Abonnement | [https://portal.azure.com](https://portal.azure.com/) | Cost Management Reader (of Reader) | **Ao-weer gave-kosten** ingeschakeld | Alle resources/resourcegroepen in het abonnement |
| Resourcegroep | [https://portal.azure.com](https://portal.azure.com/) | Cost Management Reader (of Reader) | **Ao-weer gave-kosten** ingeschakeld | Alle resources in de resourcegroep |

<sup>1</sup> het facturerings account wordt ook wel de Enterprise Agreement of inschrijving genoemd.

<sup>2</sup> het inschrijvings account wordt ook de eigenaar van het account genoemd.

In het volgende diagram ziet u de relatie tussen Cost Management bereiken met rollen en EA-Portal instellingen.

![Diagram van de relatie tussen Cost Management bereiken met rollen en EA-Portal instellingen](./media/assign-access-acm-data/scope-access-relationship-diagram.png)

Wanneer de **da-weergave kosten** zijn uitgeschakeld in de EA-Portal, ziet u een bericht waarin *de kosten voor uw organisatie zijn uitgeschakeld* wanneer u kosten voor afdelingen en accounts probeert weer te geven.

Op dezelfde manier wordt een bericht weer gegeven dat de kosten voor de registratie van *uw organisatie worden uitgeschakeld* wanneer u de kosten voor inschrijvings accounts, beheer groepen, abonnementen en resource groepen probeert weer te geven, wanneer de **ao-weergave** kosten zijn uitgeschakeld in de EA-Portal.

## <a name="other-azure-account-scopes"></a>Andere Azure-account bereiken

Voor het weer geven van kosten gegevens voor andere Azure-abonnementen moet een gebruiker ten minste lees toegang hebben tot een of meer van de volgende bereiken:

- Azure-account
- Beheergroep
- Resourcegroep

Er zijn verschillende bereiken beschikbaar na partners die klanten op de micro soft-klanten overeenkomst hebben opvolgd. CSP-klanten kunnen vervolgens Cost Management-functies gebruiken wanneer ze zijn ingeschakeld door hun CSP-partner. Zie [aan de slag met Azure Cost Management voor partners](get-started-partners.md)voor meer informatie.

## <a name="enable-access-to-costs-in-the-ea-portal"></a>Toegang tot de kosten van de EA-portal inschakelen

Voor het afdelings bereik moet de optie **voor de da-weer gave-kosten** zijn **ingeschakeld** in de EA-Portal. Voor alle andere bereiken is de optie **voor het weer geven van Ao** in de EA-Portal **ingeschakeld** .

Een optie inschakelen:

1. Meld u aan bij de EA-Portal op [https://ea.azure.com](https://ea.azure.com) met een Administrator-account voor ondernemingen.
2. Selecteer **beheren** in het linkerdeel venster.
3. Voor de cost management-bereiken waartoe u toegang wilt verlenen, schakelt u de optie kosten in op **da weer gave van kosten** en/of **ao weer geven**.  
    ![tabblad inschrijvingen opties voor de weer gave van DA-en AO](./media/assign-access-acm-data/ea-portal-enrollment-tab.png)

Nadat de opties voor de kosten zijn ingeschakeld, moeten de meeste bereiken op basis van rollen (RBAC) machtiging configuratie voor toegangsbeheer in Azure portal.

## <a name="enterprise-administrator-role"></a>Rol van Enterprise-beheerder

Een enterprise-beheerder heeft standaard toegang tot de factureringsaccount (Enterprise Agreement/inschrijving) en alle andere bereiken, die onderliggende bereiken. Toegang tot de enterprise-beheerder toegewezen aan bereiken voor andere gebruikers. Als een best practice voor bedrijfscontinuïteit, moet u altijd twee gebruikers met een enterprise-beheerderstoegang hebben. De volgende secties zijn voorbeelden van de procedure van de enterprise-beheerder toewijzen toegang tot bereiken voor andere gebruikers.

## <a name="assign-billing-account-scope-access"></a>Facturering toegang tot het bereik toewijzen

Toegang tot de facturering accountbereik is enterprise-beheerder toestemming in de EA-portal vereist. De enterprise-beheerder heeft toegang tot de kosten voor de hele EA-inschrijving of meerdere inschrijvingen weergeven. Er is geen actie is vereist in de Azure-portal voor de facturering accountbereik.

1. Meld u aan bij de EA-Portal op [https://ea.azure.com](https://ea.azure.com) met een Administrator-account voor ondernemingen.
2. Selecteer **beheren** in het linkerdeel venster.
3. Selecteer op het tabblad **inschrijving** de registratie die u wilt beheren.  
    ![Selecteer uw inschrijving in het EA-Portal](./media/assign-access-acm-data/ea-portal.png)
4. Klik op **+ beheerder toevoegen**.
5. Selecteer het verificatietype en typ de e-mailadres van de gebruiker in het vak beheerder toevoegen.
6. Als de gebruiker alleen-lezen toegang moet hebben tot kosten-en gebruiks gegevens, selecteert u **Ja**in het vak **alleen-lezen**.  Anders selecteert u **Nee**.
7. Klik op **toevoegen** om het account te maken.  
    ![voorbeeld informatie weer gegeven in het vak beheerder toevoegen](./media/assign-access-acm-data/add-admin.png)

Het duurt maximaal 30 minuten voordat de nieuwe gebruiker toegang heeft tot gegevens in Cost Management.

### <a name="assign-department-scope-access"></a>Afdeling bereik toegang toewijzen

Toegang tot het bereik van de afdeling is vereist afdeling beheerderstoegang (kosten voor het weergeven van DA) in de EA-portal. De beheerder van de afdeling heeft toegang tot de kosten en gebruiksgegevens die zijn gekoppeld met een afdeling of op meerdere afdelingen weergeven. Gegevens voor de afdeling bevatten alle abonnementen die behoren tot een inschrijvingsaccount die zijn gekoppeld aan de afdeling. Er is geen actie is vereist in de Azure-portal.

1. Meld u aan bij de EA-Portal op [https://ea.azure.com](https://ea.azure.com) met een Administrator-account voor ondernemingen.
2. Selecteer **beheren** in het linkerdeel venster.
3. Selecteer op het tabblad **inschrijving** de registratie die u wilt beheren.
4. Klik op het tabblad **afdeling** en klik vervolgens op **beheerder toevoegen**.
5. In het vak afdeling beheerder toevoegen, selecteert u het verificatietype en typ vervolgens de e-mailadres van de gebruiker.
6. Als de gebruiker alleen-lezen toegang moet hebben tot kosten-en gebruiks gegevens, selecteert u **Ja**in het vak **alleen-lezen**.  Anders selecteert u **Nee**.
7. Selecteer de afdelingen die u wilt een beheerdersmachtiging nodig voor afdeling verlenen.
8. Klik op **toevoegen** om het account te maken.  
    ![vereiste gegevens invoeren in het vak beheerder van de afdeling toevoegen](./media/assign-access-acm-data/add-depart-admin.png)

## <a name="assign-enrollment-account-scope-access"></a>Toegang tot het bereik voor inschrijving toewijzen

Toegang tot het bereik van de account inschrijving vereist account eigenaar (kosten door de AO weergeven) toegang in de EA-portal. Eigenaar van het account kan kosten- en gebruiksgegevens die zijn gekoppeld aan de abonnementen die zijn gemaakt op basis van die apparaatregistratie-account weergeven. Er is geen actie is vereist in de Azure-portal.

1. Meld u aan bij de EA-Portal op [https://ea.azure.com](https://ea.azure.com) met een Administrator-account voor ondernemingen.
2. Selecteer **beheren** in het linkerdeel venster.
3. Selecteer op het tabblad **inschrijving** de registratie die u wilt beheren.
4. Klik op het tabblad **account** en klik vervolgens op **account toevoegen**.
5. Selecteer in het vak account toevoegen de **afdeling** waaraan u het account wilt koppelen, of als niet-toegewezen.
6. Selecteer het verificatietype en typ de naam van het account.
7. Typ de e-mailadres van de gebruiker en typt u de kostenplaats (optioneel).
8. Klik op **toevoegen** om het account te maken.  
    ![vereiste gegevens invoeren in het vak account toevoegen voor een inschrijvings account](./media/assign-access-acm-data/add-account.png)

Na het voltooien van de bovenstaande stappen het gebruikersaccount dat wordt een voor apparaatregistratie-account in de Enterprise portal en -abonnementen kunt maken. De gebruiker kan toegang tot gegevens van kosten en gebruik voor abonnementen die ze maken.

## <a name="assign-management-group-scope-access"></a>Bereik toegang tot de groep toewijzen

Voor toegang tot weer gave van het bereik van de beheer groep is ten minste Cost Management de machtiging Lezer (of lezer) vereist. U kunt machtigingen voor een beheergroep configureren in Azure portal. U hebt ten minste de machtiging Administrator voor gebruikerstoegang (of eigenaar) voor de beheergroep die u wilt toegang inschakelen voor anderen. En voor Azure EA-accounts moet u ook de instelling **ao weer geven** in de EA-Portal hebben ingeschakeld.

1. Meld u aan bij de Azure Portal op [https://portal.azure.com](https://portal.azure.com).
2. Selecteer **alle services** in de zijbalk, zoek naar _beheer groepen_en selecteer vervolgens **beheer groepen**.
3. De beheergroep die u in de hiërarchie selecteren.
4. Klik naast de naam van de beheer groep op **Details**.
5. Selecteer **Access Control (IAM)** in het linkerdeel venster.
6. Klik op **Toevoegen**.
7. Selecteer **Cost Management Reader**onder **rol**.
8. Onder **toegang toewijzen aan**selecteert u **Azure AD-gebruiker,-groep of-toepassing**.
9. Toegang toewijzen, zoeken naar en selecteer vervolgens de gebruiker.
10. Klik op **Opslaan**.  
    ![voorbeeld informatie in het vak Machtigingen toevoegen voor een beheer groep](./media/assign-access-acm-data/add-permissions.png)

## <a name="assign-subscription-scope-access"></a>Abonnement bereik toegang toewijzen

Toegang tot een abonnement vereist ten minste de machtiging Cost Management Reader (of lezer). U kunt machtigingen voor een abonnement configureren in Azure portal. U hebt ten minste de machtiging Administrator voor gebruikerstoegang (of eigenaar) voor het abonnement om toegang te voor anderen. En voor Azure EA-accounts moet u ook de instelling **ao weer geven** in de EA-Portal hebben ingeschakeld.

1. Meld u aan bij de Azure Portal op [https://portal.azure.com](https://portal.azure.com).
2. Selecteer **alle services** in de zijbalk, zoek naar _abonnementen_en selecteer vervolgens **abonnementen**.
3. Selecteer uw abonnement.
4. Selecteer **Access Control (IAM)** in het linkerdeel venster.
5. Klik op **Toevoegen**.
6. Selecteer **Cost Management Reader**onder **rol**.
7. Onder **toegang toewijzen aan**selecteert u **Azure AD-gebruiker,-groep of-toepassing**.
8. Toegang toewijzen, zoeken naar en selecteer vervolgens de gebruiker.
9. Klik op **Opslaan**.

## <a name="assign-resource-group-scope-access"></a>Resource group bereik toegang toewijzen

Toegang tot een resourcegroep is vereist ten minste de machtiging Cost Management Reader (of lezer). U kunt machtigingen om een resourcegroep te configureren in Azure portal. U hebt ten minste de machtiging Administrator voor gebruikerstoegang (of eigenaar) voor de resourcegroep voor het inschakelen van toegang voor anderen. En voor Azure EA-accounts moet u ook de instelling **ao weer geven** in de EA-Portal hebben ingeschakeld.

1. Meld u aan bij de Azure Portal op [https://portal.azure.com](https://portal.azure.com).
2. Selecteer **alle services** in de zijbalk, zoek naar _resource groepen_en selecteer vervolgens **resource groepen**.
3. Selecteer de resourcegroep.
4. Selecteer **Access Control (IAM)** in het linkerdeel venster.
5. Klik op **Toevoegen**.
6. Selecteer **Cost Management Reader**onder **rol**.
7. Onder **toegang toewijzen aan**selecteert u **Azure AD-gebruiker,-groep of-toepassing**.
8. Toegang toewijzen, zoeken naar en selecteer vervolgens de gebruiker.
9. Klik op **Opslaan**.

## <a name="cross-tenant-authentication-issues"></a>Problemen met verificatie tussen tenants

Momenteel heeft Azure Cost Management beperkte ondersteuning voor verificatie tussen tenants. In sommige gevallen is het mogelijk dat er een fout bericht over **geweigerde toegang** wordt weer gegeven in de kosten analyse. Dit probleem kan optreden als u RBAC (op rollen gebaseerd toegangs beheer) configureert voor het abonnement van een andere Tenant en vervolgens probeert de kosten gegevens weer te geven.

*Het probleem*omzeilen: nadat u RBAC tussen tenants hebt geconfigureerd, wacht u een uur. Probeer vervolgens de kosten in kosten analyse weer te geven of Cost Management toegang te verlenen aan gebruikers in beide tenants.  


## <a name="next-steps"></a>Volgende stappen

- Als u de eerste Snelstartgids voor Cost Management nog niet hebt voltooid, kunt u deze lezen bij het analyseren van de [kosten](quick-acm-cost-analysis.md).
