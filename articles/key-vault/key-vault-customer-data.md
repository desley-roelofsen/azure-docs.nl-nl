---
title: Azure Key Vault functies voor klant gegevens-Azure Key Vault | Microsoft Docs
description: Meer informatie over klant gegevens in Key Vault
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.topic: reference
ms.date: 01/07/2019
ms.author: mbaldwin
ms.openlocfilehash: 67e1aeab4211249075b51bd0138d7875756a3483
ms.sourcegitcommit: 7c5a2a3068e5330b77f3c6738d6de1e03d3c3b7d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/11/2019
ms.locfileid: "70883318"
---
# <a name="azure-key-vault-customer-data-features"></a>Azure Key Vault klant gegevens functies

Azure Key Vault ontvangt klant gegevens tijdens het maken of bijwerken van kluizen, sleutels, geheimen, certificaten en beheerde opslag accounts. Deze klant gegevens zijn direct zichtbaar in de Azure Portal en via de REST API. Klant gegevens kunnen worden bewerkt of verwijderd door het object dat de gegevens bevat, bij te werken of te verwijderen.

Systeem toegangs logboeken worden gegenereerd wanneer een gebruiker of toepassing toegang heeft tot Key Vault. Gedetailleerde toegangs logboeken zijn beschikbaar voor klanten die Azure Insights gebruiken.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

## <a name="identifying-customer-data"></a>Klant gegevens identificeren

Met de volgende informatie worden klant gegevens binnen Azure Key Vault geïdentificeerd:

- Toegangs beleid voor Azure Key Vault bevat object-Id's die gebruikers, groepen of toepassingen vertegenwoordigen
- Certificaat houders kunnen e-mail adressen of andere gebruikers-of organisatie-id's bevatten
- Certificaat contactpersonen kunnen e-mail adressen, namen of telefoon nummers van gebruikers bevatten
- Certificaat verleners kunnen e-mail adressen, namen, telefoon nummers, account referenties en organisatie gegevens bevatten
- Wille keurige Tags kunnen worden toegepast op objecten in Azure Key Vault. Tot deze objecten behoren kluizen, sleutels, geheimen, certificaten en opslag accounts. De gebruikte labels bevatten mogelijk persoons gegevens
- Azure Key Vault Access-logboeken bevatten object-Id's, [upn's](../active-directory/hybrid/plan-connect-userprincipalname.md)en IP-adressen voor elke rest API-aanroep
- Azure Key Vault Diagnostische logboeken kunnen object-Id's en IP-adressen voor REST API-aanroepen bevatten

## <a name="deleting-customer-data"></a>Verwijderen van klantgegevens

Dezelfde REST Api's, portal-ervaring en Sdk's die worden gebruikt voor het maken van kluizen, sleutels, geheimen, certificaten en beheerde opslag accounts, kunnen ook deze objecten bijwerken en verwijderen.

Met de functie voor voorlopig verwijderen kunt u na het verwijderen van 90 dagen verwijderde gegevens herstellen. Bij het gebruik van zacht verwijderen, kunnen de gegevens permanent worden verwijderd voordat de Bewaar periode van 90 dagen is verlopen door een opschoon bewerking uit te voeren. Als de kluis of het abonnement is geconfigureerd voor het blok keren van leegmaak bewerkingen, is het niet mogelijk om gegevens definitief te verwijderen totdat de geplande Bewaar periode is verstreken.

## <a name="exporting-customer-data"></a>Exporteren van gegevens van de klant

Met dezelfde REST Api's, portal-ervaring en Sdk's die worden gebruikt voor het maken van kluizen, sleutels, geheimen, certificaten en beheerde opslag accounts kunt u ook deze objecten weer geven en exporteren.

Azure Key Vault Access-logboek registratie is een optionele functie die kan worden ingeschakeld om logboeken voor elke REST API-aanroep te genereren. Deze logboeken worden overgebracht naar een opslag account in uw abonnement waar u het Bewaar beleid toepast dat voldoet aan de vereisten van uw organisatie.

Azure Key Vault Diagnostische logboeken die persoonlijke gegevens bevatten, kunnen worden opgehaald door een export aanvraag te maken in de gebruikers-privacy-Portal. Deze aanvraag moet worden ingediend door de Tenant beheerder.

## <a name="next-steps"></a>Volgende stappen

- [Logboekregistratie van Azure Key Vault](key-vault-logging.md)

- [Overzicht van Azure Key Vault voorlopig verwijderen](key-vault-soft-delete-cli.md)

- [Sleutel bewerkingen Azure Key Vault](https://docs.microsoft.com/rest/api/keyvault/key-operations)

- [Azure Key Vaulte geheime bewerkingen](https://docs.microsoft.com/rest/api/keyvault/secret-operations)

- [Certificaten en beleids regels Azure Key Vault](https://docs.microsoft.com/rest/api/keyvault/certificates-and-policies)

- [Bewerkingen voor opslag accounts Azure Key Vault](https://docs.microsoft.com/rest/api/keyvault/storage-account-key-operations)
