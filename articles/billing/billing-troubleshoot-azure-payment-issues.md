---
title: Problemen met Azure-betalingen oplossen
description: Een probleem oplossen met het bijwerken van het betalingsgegevensaccount in de Microsoft Azure-portal of het Azure-accountcentrum.
services: azure
author: v-miegge
manager: dcscontentpm
editor: v-jesits
tags: billing
ms.service: cost-management-billing
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/12/2019
ms.author: jaserano
ms.openlocfilehash: b449dbf455807aab8436f42c25f21f3eea43a678
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/20/2019
ms.locfileid: "74225849"
---
# <a name="troubleshoot-azure-payment-issues"></a>Problemen met Azure-betalingen oplossen

Er treedt mogelijk een fout of probleem op wanneer u het betalingsgegevensaccount probeert te bewerken in de Microsoft Azure-portal of in het Azure-accountcentrum.

Als u het probleem wilt oplossen, selecteert u hieronder het onderwerp dat het meest op uw probleem lijkt.

## <a name="unable-to-remove-a-credit-card-from-a-saved-billing-payment-method"></a>Kan creditcard niet verwijderen uit een opgeslagen betalingswijze

U kunt de creditcard standaard niet verwijderen uit het actieve abonnement.

Als u een bestaande kaart wilt verwijderen, moet er een nieuwe kaart aan het abonnement worden toegevoegd, zodat het oude betaalmiddel kan worden verwijderd. U kunt ook het abonnement annuleren. Hiermee verwijdert u het abonnement permanent en verwijdert u ook de kaart.

## <a name="unable-to-delete-an-old-payment-method-after-adding-a-new-payment-method"></a>Kan een oude betalingswijze niet verwijderen nadat een nieuwe betalingswijze is toegevoegd

Het nieuwe betaalmiddel is mogelijk niet gekoppeld aan het abonnement. Zie [Een creditcard of debitcard voor Azure toevoegen, bijwerken of verwijderen](billing-how-to-change-credit-card.md) voor meer informatie over het koppelen van het betaalmiddel aan het abonnement.

Zie [Problemen met een geweigerde kaart bij Azure-aanmelding oplossen](billing-troubleshoot-declined-card.md) voor meer informatie over het oplossen van problemen met een geweigerde kaart.

## <a name="unable-to-delete-a-payment-method-because-of-cannot-delete-payment-method-error"></a>Kan betalingswijze niet verwijderen door het foutbericht *Kan betalingswijze niet verwijderen*

Dit komt door een openstaand saldo. Betaal openstaande saldo's voordat u de betalingswijze verwijdert.

## <a name="unable-to-see-subscriptions-under-my-account-to-update-the-payment-method"></a>Ik zie geen abonnementen in mijn account om de betalingswijze bij te werken

U gebruikt mogelijk een e-mail-id die afwijkt van de naam die wordt gebruikt voor de abonnementen.

Als u dit probleem wilt oplossen, raadpleegt u [Aanmeldingsfout dat er geen abonnementen zijn gevonden voor Azure Portal of het Azure-accountcentrum](billing-no-subscriptions-found.md).

## <a name="unable-to-make-payment-for-a-subscription"></a>Kan geen betaling doen voor een abonnement

Als u het volgende foutbericht krijgt: *De betaling is te laat. Er is een probleem met uw betalingswijze* of *De informatie kan niet worden opgeslagen. Sluit de browser en probeer het opnieuw*, is er een betaling die op de kaart in behandeling is, omdat de kaart is geweigerd door uw financiële instelling.

Controleer of het saldo van de creditcard voldoende is om een betaling te doen. Als dat niet het geval is, gebruikt u een andere kaart om de betaling te doen of neemt u contact op met uw financiële instelling om het probleem op te lossen.

Neem contact op met uw bank voor de volgende problemen:

- Internationale transacties zijn niet ingeschakeld.
- De kaart heeft een kredietlimiet en het saldo moet worden vereffend.
- Er is een terugkerende betaling ingeschakeld op de kaart.

## <a name="unable-to-change-payment-method-because-of-browser-issues-browser-does-not-respond-does-not-load-and-so-on"></a>De betalingswijze kan niet worden gewijzigd vanwege browserproblemen (de browser reageert niet, wordt niet geladen enzovoort)

Meld u af bij alle actieve Azure-sessies en volg de stappen in het artikel [InPrivate-browsing in Microsoft Edge](https://support.microsoft.com/help/4026200/microsoft-edge-browse-inprivate) om een InPrivate-sessie in Microsoft Edge of Internet Explorer te starten.

Volg in de privésessie de stappen voor [Een creditcard wijzigen](billing-how-to-change-credit-card.md) om de creditcardgegevens bij te werken of te wijzigen.

U kunt ook de volgende handelingen uitvoeren:

- Uw browser vernieuwen
- Een andere browser gebruiken
- Cookies in de cache verwijderen

## <a name="my-subscription-is-still-disabled-after-updating-the-payment-method"></a>Mijn abonnement is nog steeds uitgeschakeld nadat de betalingswijze is bijgewerkt.

Dit probleem treedt op vanwege een openstaand saldo. Betaal openstaande saldo's voordat u de betalingswijze verwijdert.

## <a name="unable-to-change-payment-method-because-of-an-xml-error-response-page"></a>Kan de betalingswijze niet wijzigen vanwege een responspagina met een XML-fout

U ontvangt dit bericht als u [Azure Portal](https://portal.azure.com/) gebruikt om een nieuwe creditcard toe te voegen.

U moet u aanmelden bij de Azure-accountportal met het e-mailadres van de accountbeheerder om kaartgegevens toe te voegen.

## <a name="additional-help-resources"></a>Aanvullende ondersteuningsresources

Andere artikelen met probleemoplossingen voor Azure Billing en abonnementen

- [Geweigerde kaart](billing-troubleshoot-declined-card.md)
- [Aanmeldingsproblemen voor abonnementen](billing-troubleshoot-sign-in-issue.md)
- [Geen abonnementen gevonden](billing-no-subscriptions-found.md)
- [Weergave voor bedrijfskosten is uitgeschakeld](billing-enterprise-mgmt-grp-troubleshoot-cost-view.md)

## <a name="contact-us-for-help"></a>Contact met ons opnemen voor ondersteuning

Als u vragen hebt of hulp nodig hebt, [kunt u een ondersteuningsaanvraag maken](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

## <a name="next-steps"></a>Volgende stappen

- [Documentatie voor Azure Billing](index.md)
