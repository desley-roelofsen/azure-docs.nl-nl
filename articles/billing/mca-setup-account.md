---
title: Uw factureringsrekening configureren voor een Microsoft-klantovereenkomst - Azure
description: Informatie over hoe u uw factureringsrekening configureert voor een Microsoft-klantovereenkomst.
author: amberbhargava
manager: amberb
editor: banders
tags: billing
ms.service: cost-management-billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/01/2019
ms.author: banders
ms.openlocfilehash: 5d01fa5f4bb7367bdd1939d77dc1f80bd4436765
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/20/2019
ms.locfileid: "74225581"
---
# <a name="set-up-your-billing-account-for-a-microsoft-customer-agreement"></a>Uw factureringsrekening configureren voor een Microsoft-klantovereenkomst

Als uw Enterprise Agreement-inschrijving is verlopen of binnenkort verloopt, kunt u een Microsoft-klantovereenkomst ondertekenen om uw inschrijving te vernieuwen. In dit artikel worden de wijzigingen aan uw bestaande facturering na de configuratie beschreven en wordt u door de configuratie van uw nieuwe factureringsrekening geleid. De vernieuwing bestaat uit de volgende stappen:

1. De nieuwe Microsoft-klantovereenkomst accepteren. Met uw Microsoft-vertegenwoordiger samenwerken om de details van de nieuwe overeenkomst te bespreken en te accepteren.
2. De nieuwe factureringsrekening configureren die is gemaakt voor de nieuwe Microsoft-klantovereenkomst.

Als u de factureringsrekening wilt configureren, moet u de facturering van uw Azure-abonnementen overzetten van uw Enterprise Agreement-inschrijving naar de nieuwe rekening. De configuratie is niet van invloed op Azure-services die worden uitgevoerd in uw abonnementen. Het is echter wel van invloed op de manier waarop u de facturering voor uw abonnementen beheert.

- In plaats van de [EA-portal](https://ea.azure.com) beheert u uw Azure-services en -facturering in de [Azure-portal](https://portal.azure.com).
- U ontvangt een maandelijkse digitale factuur voor uw kosten. U bekijkt de factuur op de pagina Azure Cost Management en facturering.
- In plaats van afdelingen en accounts in uw Enterprise Agreement-inschrijving gebruikt u de factureringsstructuur en -scopes van de nieuwe rekening om uw facturering te beheren en ordenen.

Voordat u aan de configuratie begint, raden we aan dat u het volgende doet:

- **Inzicht krijgen in uw nieuwe factureringsrekening**
  - Uw nieuwe rekening vereenvoudigt facturering voor uw organisatie. [Krijg een snel overzicht van uw nieuwe factureringsrekening](billing-mca-overview.md)
- **Uw toegang controleren om de configuratie af te ronden**
  - Alleen gebruikers met bepaalde administratieve machtigingen kunnen de configuratie afronden. Controleer of u [de vereiste toegang hebt om de configuratie af te ronden](#access-required-to-complete-the-setup).
- **Inzicht krijgen in de wijzigingen in uw factureringshiërarchie**
  - Uw nieuwe factureringsrekening is anders ingedeeld dan uw Enterprise Agreement-inschrijving. [Krijgen inzicht in de wijzigingen in uw factureringshiërarchie in de nieuwe rekening](#understand-changes-to-your-billing-hierarchy).
- **Wijzigingen in de toegang van uw factureringsbeheerder begrijpen**
  - Beheerders van uw Enterprise Agreement-inschrijving krijgen in de nieuwe rekening toegang tot de factureringsbereiken. [Leer de wijzigingen in hun toegang kennen](#changes-to-billing-administrator-access).
- **Enterprise Agreement-functies bekijken die worden vervangen door de nieuwe rekening**
  - Bekijk de functies van de Enterprise Agreement-inschrijving die worden vervangen door functies in de nieuwe rekening.
- **Antwoorden bekijken op de meest gestelde vragen**
  - Raadpleeg [aanvullende informatie](#additional-information) voor meer informatie over de configuratie.

## <a name="access-required-to-complete-the-setup"></a>Vereiste toegang om de configuratie af te ronden

U hebt de volgende toegang nodig om de configuratie af te ronden:

- Eigenaar van het factureringsprofiel dat is gemaakt toen de Microsoft-klantovereenkomst werd ondertekend. Raadpleeg [Inzicht in factureringsprofielen](billing-mca-overview.md#billing-profiles) voor meer informatie over factureringsprofielen.

- Ondernemingsbeheerder van de inschrijving die is vernieuwd.

### <a name="if-youre-not-an-enterprise-administrator-on-the-enrollment"></a>Als u tijdens de inschrijving geen ondernemingsbeheerder bent

U kunt de ondernemingsbeheerders van de inschrijving vragen de configuratie van uw factureringsrekening af te ronden.

1. Meld u aan bij de Azure-portal met de link in de e-mail die u hebt ontvangen toen u de Microsoft-klantovereenkomst ondertekende.

2. Als u deze e-mail niet hebt ontvangen, meldt u zich aan met de volgende link. Vervang `<enrollmentNumber>` door het inschrijvingsnummer van uw Enterprise Agreement die is vernieuwd.

   `https://portal.azure.com/#blade/Microsoft_Azure_EA/EATransitionToMCA/enrollmentId/<enrollmentNumber>`

3. Selecteer de ondernemingsbeheerders die u het verzoek wilt sturen.

   ![Schermafbeelding van een uitnodiging voor de ondernemingsbeheerders](./media/mca-setup-account/ea-mca-invite-admins.png)

4. Selecteer **Verzoek verzenden**.

   De beheerders ontvangen een e-mail met instructies om de configuratie af te ronden.

### <a name="if-youre-not-an-owner-of-the-billing-profile"></a>Als u geen eigenaar bent van een factureringsprofiel

De gebruiker in uw organisatie die de Microsoft-klantovereenkomst heeft ondertekend, wordt toegevoegd als eigenaar van het factureringsprofiel. Vraag de gebruiker om u toe te voegen als eigenaar, zodat u de configuratie kunt afronden.

## <a name="understand-changes-to-your-billing-hierarchy"></a>Wijzigingen in uw factureringshiërarchie begrijpen

Uw nieuwe factureringsrekening vereenvoudigt de facturering voor uw organisatie en geeft u verbeterde mogelijkheden voor facturering en kostenbeheer. In het volgende diagram wordt uitgelegd hoe facturering is georganiseerd in de nieuwe factureringsrekening.

![Afbeelding van ea-mca-post-transition-hierarchy](./media/mca-setup-account/mca-post-transition-hierarchy.png)

1. U gebruikt de factureringsrekening om de facturering voor uw Microsoft-klantovereenkomst te beheren. Enterprise-beheerders worden eigenaar van de factureringsrekening. Raadpleeg [Inzicht in de factureringsrekening](billing-mca-overview.md#your-billing-account) voor meer informatie over de factureringsrekening.
2. U gebruikt het factureringsprofiel om de facturering voor uw organisatie te beheren, vergelijkbaar met uw Enterprise Agreement-inschrijving. Ondernemingsbeheerders worden eigenaar van het factureringsprofiel. Raadpleeg [Inzicht in factureringsprofielen](billing-mca-overview.md#billing-profiles) voor meer informatie over factureringsprofielen.
3. U gebruikt een factuursectie om uw kosten te ordenen op basis van uw behoeften, net als de afdelingen in uw Enterprise Agreement-inschrijving. Afdelingen worden factuursecties en afdelingsbeheerders worden eigenaar van de respectieve factuursecties. Raadpleeg [inzicht in factuursecties](billing-mca-overview.md#invoice-sections) voor meer informatie over factuursecties.
4. De accounts die in uw Enterprise Agreement zijn gemaakt, worden niet ondersteund in de nieuwe factureringsrekening. De abonnementen van het account horen bij de respectieve factuursectie voor hun afdeling. Accounteigenaren kunnen abonnementen voor hun factuursecties maken en beheren.

## <a name="changes-to-billing-administrator-access"></a>Wijzigingen in de toegang van de factureringsbeheerder

Afhankelijk van hun toegang krijgen factureringsbeheerders in uw Enterprise Agreement-inschrijving toegang tot de factureringsbereiken in de nieuwe rekening. In de volgende tabel worden de wijzigingen in toegang tijdens de configuratie uitgelegd:

| Bestaande rol | Rol na de overgang |
| --- | --- |
| **Ondernemingsbeheerder (alleen-lezen = nee)** | **- Eigenaar van factureringsrekening** </br> Beheert alles in de factureringsrekening </br> **- Eigenaar van het factureringsprofiel** </br> Beheert alles in het factureringsprofiel </br> **- Factuursectie-eigenaar van alle factuursecties** </br> Beheert alles in de factuursecties |
| **Ondernemingsbeheerder (alleen-lezen = ja)** | **- Lezer van factureringsrekening** </br> Alleen-lezen weer gave van alles voor de factureringsrekening</br> **- Lezer van factureringsprofiel** </br> Alleen-lezen weer gave van alles voor het factureringsprofiel</br>**- Factuursectie-lezer van alle factuursecties**</br> Alleen-lezen weergave van alles in de factuursecties|
| **Afdelingsbeheerder (alleen-lezen = nee)** |**- Factuursectie-eigenaar van de factuursectie die is gemaakt voor de respectieve afdeling** </br>Beheert alles in de factuursectie|
| **Afdelingsbeheerder (alleen-lezen = ja)**|**- Factuursectie-lezer van de factuursectie die is gemaakt voor de respectieve afdeling**</br> Alleen-lezen weergave van alles voor de factuursectie|
| **Accounteigenaar** | **- Maker van het Azure-abonnement in de factuursectie van de respectieve afdeling** </br>  Maakt Azure-abonnementen voor zijn factuursectie|

Er wordt een Azure AD-tenant (Active Directory) geselecteerd voor de nieuwe factureringsrekening wanneer u akkoord gaat met de Microsoft-klantovereenkomst. Als er geen tenant bestaat voor uw organisatie, wordt er een nieuwe tenant gemaakt. De tenant vertegenwoordigt uw organisatie binnen Azure Active Directory. Globale tenantbeheerders in uw organisatie gebruiken de tenant om toegang te beheren tot toepassingen en gegevens in uw organisatie.

Uw nieuwe rekening ondersteunt alleen gebruikers van de tenant die is geselecteerd tijdens het ondertekenen van de Microsoft-klantovereenkomst. Als gebruikers met administratieve machtigingen in uw Enterprise Agreement onderdeel zijn van de tenant, krijgen ze tijdens de configuratie toegang tot de nieuwe factureringsrekening. Als ze geen deel uitmaken van de tenant hebben ze geen toegang tot de nieuwe factureringsrekening, tenzij u hen uitnodigt.

Wanneer u de gebruikers uitnodigt, worden ze als gastgebruikers toegevoegd aan de tenant en krijgen ze toegang tot de factureringsrekening. Als u de gebruikers wilt uitnodigen, moet gasttoegang zijn uitgeschakeld voor de tenant. Raadpleeg [gasttoegang beheren in Azure Active Directory](https://docs.microsoft.com/microsoftteams/teams-dependencies#control-guest-access-in-azure-active-directory) voor meer informatie. Als gasttoegang is uitgeschakeld, neemt u contact op met de globale beheerders van uw tenant om toegang in te schakelen. <!-- Todo - How can they find their global administrator -->

## <a name="view-replaced-features"></a>Vervangen functies weergeven

De volgende Enterprise Agreement-functies worden vervangen door nieuwe functies in de factureringsrekening voor een Microsoft-klantovereenkomst.

### <a name="enterprise-agreement-accounts"></a>Enterprise Agreement-accounts

De accounts die zijn gemaakt in uw Enterprise Agreement-inschrijving worden niet ondersteund in de nieuwe factureringsrekening. De abonnementen van het account horen bij de factuursectie die is gemaakt voor de respectieve afdeling. Accounteigenaren worden makers van Azure-abonnementen en kunnen abonnement voor hun factuursecties maken en beheren.

### <a name="notification-contacts"></a>Contactpersonen voor meldingen

Contactpersonen voor meldingen ontvangen e-mails over de Azure Enterprise Agreement. Ze worden niet ondersteund in de nieuwe factureringsrekening. E-mails over Azure-tegoed en facturen worden naar gebruikers gestuurd die toegang hebben tot de factureringsprofielen in uw factureringsrekening.

### <a name="spending-quotas"></a>Bestedingsquota

Bestedingsquota die zijn ingesteld voor de afdelingen in uw Enterprise Agreement-inschrijving worden vervangen door budgetten in de nieuwe factureringsrekening. Er wordt een budget gemaakt voor elk bestedingsquotum in afdelingen in uw inschrijving. Voor meer informatie over budgetten raadpleegt u [Azure-budgetten maken en beheren](../cost-management/manage-budgets.md).

### <a name="cost-centers"></a>Kostenplaatsen

Kostenplaatsen die zijn ingesteld in de Azure-abonnementen in uw Enterprise Agreement-inschrijving worden overgedragen naar de nieuwe factureringsrekening. Kostenplaatsen voor afdelingen en Enterprise Agreement-accounts worden echter niet ondersteund.

## <a name="additional-information"></a>Aanvullende informatie

In de volgende secties vindt u meer informatie over de configuratie van uw factureringsrekening.

### <a name="no-service-downtime"></a>Geen servicedowntime

Azure-services in uw abonnement worden zonder onderbreking uitgevoerd. We zetten alleen de factureringsrelatie voor uw Azure-abonnementen over. Dit heeft geen invloed op bestaande resources, resourcegroepen of beheergroepen.

### <a name="user-access-to-azure-resources"></a>Gebruikerstoegang tot Azure-resources

Toegang tot Azure-resources die is ingesteld met Azure RBAC (op rollen gebaseerd toegangsbeheer) wordt niet beïnvloed tijdens de overgang.

### <a name="azure-reservations"></a>Azure-reserveringen

Alle Azure-reserveringen in uw Enterprise Overeenkomst-inschrijving worden verplaatst naar uw nieuwe factureringsrekening. Tijdens de overgang worden er geen wijzigingen aangebracht in de reserveringskortingen die worden toegepast op uw abonnementen.

### <a name="azure-marketplace-products"></a>Azure Marketplace-producten

Alle Azure Marketplace-producten in uw Enterprise Agreement-inschrijving worden samen met de abonnementen verplaatst. Er zijn geen wijzigingen in de servicetoegang van de Marketplace-producten tijdens de overgang.

### <a name="support-plan"></a>Ondersteuningsplan

Ondersteuningsvoordelen worden niet overgedragen tijdens de overgang. Schaf een nieuwe ondersteuningsplan aan voor voordelen voor uw Azure-abonnementen in uw nieuwe factureringsrekening.

### <a name="past-charges-and-balance"></a>Eerdere kosten en eerder saldo

Kosten en tegoedsaldo voor de overgang kunnen worden bekeken in uw Enterprise Agreement-inschrijving via de Azure-portal. <!--Todo - Add a link for this-->

### <a name="when-should-the-setup-be-completed"></a>Wanneer moet de configuratie worden afgerond?

Rond de configuratie van uw factureringsrekening af voordat uw Enterprise Agreement-inschrijving verloopt. Als uw inschrijving verloopt, blijven services in uw Azure-abonnementen worden uitgevoerd zonder onderbreking. Er worden echter tarieven voor betalen per gebruik in rekening gebracht voor de services.

### <a name="changes-to-the-enterprise-agreement-enrollment-after-the-setup"></a>Wijzigingen in de Enterprise Agreement-inschrijving na de configuratie

Azure-abonnementen die voor de Enterprise Agreement-inschrijving zijn gemaakt, kunnen na de overgang handmatig worden verplaatst naar de nieuwe factureringsrekening. Raadpleeg [factureringseigendom krijgen van Azure-abonnementen van andere gebruikers](billing-mca-request-billing-ownership.md) voor meer informatie. Als a Azure-reserveringen wilt verplaatsen die na de overgang zijn aangeschaft, [neemt u contact op met de Azure-ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). U kunt ook na de overgang gebruikerstoegang toewijzen aan de factureringsrekening. Raadpleeg [Factureringsrollen beheren in de Azure-portal](billing-understand-mca-roles.md#manage-billing-roles-in-the-azure-portal) voor meer informatie

### <a name="revert-the-transition"></a>De overgang terugdraaien

De overgang kan niet worden teruggedraaid. Nadat de facturering van uw Azure-abonnementen is overgezet naar de nieuwe factureringsrekening, kunt u deze niet terugzetten naar uw Enterprise Overeenkomst-inschrijving.

### <a name="closing-your-browser-during-setup"></a>Uw browser sluiten tijdens de installatie

U kunt de browser sluiten voordat u op **Overgang starten** klikt. U kunt teruggaan naar de configuratie via de link die u in de e-mail hebt gekregen en de overgang beginnen. Als u de browser sluit nadat de overgang is begonnen, blijft de overgang actief. Ga terug naar de pagina met de overgangsstatus om de actueelste status van uw overgang te bekijken. U ontvangt een e-mail wanneer de overgang is afgerond.

## <a name="complete-the-setup-in-the-azure-portal"></a>De configuratie afronden in de Azure-portal

Als u de configuratie wilt afronden, hebt u toegang nodig tot zowel de nieuwe factureringsrekening als de Enterprise Agreement-inschrijving. Raadpleeg [toegang vereist om de configuratie van uw factureringsrekening af te ronden](#access-required-to-complete-the-setup) voor meer informatie.

1. Meld u aan bij de Azure-portal met de link in de e-mail die u hebt ontvangen toen u de Microsoft-klantovereenkomst ondertekende.

2. Als u deze e-mail niet hebt ontvangen, meldt u zich aan met de volgende link. Vervang `<enrollmentNumber>` door het inschrijvingsnummer van uw Enterprise Agreement die is vernieuwd.

   `https://portal.azure.com/#blade/Microsoft_Azure_EA/EATransitionToMCA/enrollmentId/<enrollmentNumber>`

3. Selecteer **Overgang starten** in de laatste stap van de installatie. Nadat u de overgang hebt gestart:

    ![Schermopname van de configuratiewizard](./media/mca-setup-account/ea-mca-set-up-wizard.png)

    - In de nieuwe factureringsrekening wordt een factureringshiërarchie gemaakt die correspondeert met uw Enterprise Agreement-hiërarchie. Raadpleeg [inzicht in uw factureringshiërarchie begrijpen](#understand-changes-to-your-billing-hierarchy) voor meer informatie.
    - Beheerders van uw Enterprise Agreement-inschrijving krijgen toegang tot de nieuwe factureringsrekening, zodat zij de facturering voor uw organisatie kunnen blijven beheren.
    - De facturering van uw Azure-abonnement wordt overgezet naar het nieuwe account. **Er zijn tijdens deze overgang geen gevolgen voor uw Azure-Services. Ze blijven zonder onderbrekingen actief**.
    - Als u Azure-reserveringen hebt, worden die verplaatst naar uw nieuwe factureringsrekening met dezelfde voordelen en termijn. 

4. U kunt de status van de overgang controleren op de pagina **Overgangsstatus**.

   ![Schermopname van de overgangsstatus](./media/mca-setup-account/ea-mca-set-up-status.png)

## <a name="validate-billing-account-setup"></a>De configuratie van de factureringsrekening valideren

 Valideer het volgende om ervoor te zorgen dat uw nieuwe factureringsrekening correct is geconfigureerd:

### <a name="azure-subscriptions"></a>Azure-abonnementen

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

2. Zoek naar **Kostenbeheer en facturering**.

   ![Schermopname van de zoekopdracht in de Azure-portal](./media/mca-setup-account/search-cmb.png)

3. Selecteer het factureringsaccount. Het type factureringsrekening is **Microsoft-klantovereenkomst**.

4. Selecteer **Azure-abonnementen** aan de linkerzijde.

   ![Schermopname met de lijst abonnementen](./media/mca-setup-account/mca-subscriptions-post-transition.png)

Azure-abonnementen die zijn overgezet van uw Enterprise Agreement-inschrijving naar de nieuwe factureringsrekening worden weergegeven op de pagina Azure-abonnementen. Als u denkt dat er een abonnement ontbreekt, kunt u de facturering van het abonnement handmatig overzetten in de Azure-portal. Raadpleeg [factureringseigendom krijgen van Azure-abonnementen van andere gebruikers](billing-mca-request-billing-ownership.md) voor meer informatie

### <a name="azure-reservations"></a>Azure-reserveringen

Alle Azure-reserveringen in uw Enterprise Agreement-inschrijving worden verplaatst naar uw nieuwe factureringsrekening met dezelfde voordelen en termijn. Transacties die zijn voltooid vóór de overgang worden niet weergegeven in uw nieuwe factureringsrekening. U kunt echter controleren of de voordelen van uw reserveringen worden toegepast op uw abonnementen via de pagina [Azure-reserveringen ](https://portal.azure.com/#blade/Microsoft_Azure_Reservations/ReservationsBrowseBlade).

### <a name="access-of-enterprise-administrators-on-the-billing-account"></a>Toegang van ondernemingsbeheerders tot de factureringsrekening

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

2. Zoek naar **Kostenbeheer en facturering**.

   ![Schermopname van de zoekopdracht in de Azure-portal](./media/mca-setup-account/search-cmb.png)

3. Selecteer de factureringsrekening voor uw **Microsoft-klantovereenkomst**.

4. Selecteer **Toegangsbeheer (IAM)** aan de linkerzijde.

   ![Schermopname met toegang van ondernemingsbeheerders na de overgang](./media/mca-setup-account/mca-ea-admins-ba-access-post-transition.png)

Ondernemingsbeheerders worden vermeld als eigenaar van de factureringsrekening terwijl de ondernemingsbeheerders met alleen-lezen machtigingen worden vermeld als lezers van de factureringsrekening. Als u denkt dat toegang voor een ondernemingsbeheerder ontbreekt, geeft u hen toegang in de Azure-portal. Raadpleeg [Factureringsrollen beheren in de Azure-portal](billing-understand-mca-roles.md#manage-billing-roles-in-the-azure-portal) voor meer informatie.

### <a name="access-of-enterprise-administrators-on-the-billing-profile"></a>Toegang van ondernemingsbeheerders tot het factureringsprofiel

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

2. Zoek naar **Kostenbeheer en facturering**.

   ![Schermopname van de zoekopdracht in de Azure-portal](./media/mca-setup-account/search-cmb.png)

3. Selecteer het factureringsprofiel dat voor uw inschrijving is gemaakt. Afhankelijk van uw toegang moet u mogelijk een factureringsaccount selecteren. Selecteer in de factureringsrekening Factureringsprofielen en vervolgens het factureringsprofiel.

4. Selecteer **Toegangsbeheer (IAM)** aan de linkerzijde.

   ![Schermopname met toegang van ondernemingsbeheerders na de overgang](./media/mca-setup-account/mca-ea-admins-bp-access-post-transition.png)

Ondernemingsbeheerders worden vermeld als eigenaar van factureringsprofielen terwijl de ondernemingsbeheerders met alleen-lezenmachtiging worden vermeld als factureringsprofiellezers. Als u denkt dat toegang voor een ondernemingsbeheerder ontbreekt, geeft u hen toegang in de Azure-portal. Raadpleeg [Factureringsrollen beheren in de Azure-portal](billing-understand-mca-roles.md#manage-billing-roles-in-the-azure-portal) voor meer informatie.

### <a name="access-of-enterprise-administrators-department-administrators-and-account-owners-on-invoice-sections"></a>Toegang van ondernemingsbeheerders, afdelingsbeheerders en accounteigenaren in factuursecties

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

2. Zoek naar **Kostenbeheer en facturering**.

   ![Schermopname van de zoekopdracht in de Azure-portal](./media/mca-setup-account/search-cmb.png).

3. Selecteer een factureringsgedeelte. Factuursecties hebben dezelfde naam als hun respectieve afdelingen in Enterprise Agreement-inschrijvingen. Afhankelijk van uw toegang moet u mogelijk een factureringsaccount selecteren. Selecteer in de factureringsrekening **Factureringsprofielen** en selecteer vervolgens **Factuursecties**. Selecteer een factuursectie in de lijst Factuursecties.

   ![Schermopname van een lijst van de factuursectie na de overgang](./media/mca-setup-account/mca-invoice-sections-post-transition.png)

4. Selecteer **Toegangsbeheer (IAM)** aan de linkerzijde.

    ![Schermopname met de toegang van afdeling- en accountbeheerders na de overgang](./media/mca-setup-account/mca-department-account-admins-access-post-transition.png)

Ondernemingsbeheerders en afdelingsbeheerders worden vermeld als eigenaar van een factuursectie of lezer van een factuursectie, terwijl accounteigenaren in de afdelingen worden vermeld als makers van Azure-abonnementen. Herhaal de stap voor alle factuursecties om de toegang voor alle afdelingen in uw Enterprise Agreement-inschrijving te controleren. Accounteigenaren die geen onderdeel waren van een afdeling krijgen machtiging voor een factuursectie met de naam **Standaardfactuursectie**. Als u denkt dat toegang voor een beheerder ontbreekt, geeft u hen toegang in de Azure-portal. Raadpleeg [Factureringsrollen beheren in de Azure-portal](billing-understand-mca-roles.md#manage-billing-roles-in-the-azure-portal) voor meer informatie.

## <a name="need-help-contact-support"></a>Hulp nodig? Contact opnemen met ondersteuning

Als u hulp nodig hebt, kunt u [contact opnemen met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om uw probleem snel op te lossen.

## <a name="next-steps"></a>Volgende stappen

- [Aan de slag met uw nieuwe factureringsrekening](billing-mca-overview.md)

- [Enterprise Agreement-taken in uw factureringsrekening afronden voor een Microsoft-klantovereenkomst](billing-mca-enterprise-operations.md)

- [Toegang tot uw factureringsrekening beheren](billing-understand-mca-roles.md)
