---
title: Gebruikers toegang tot gegevens bewerkingen beperken met Azure Cosmos DB
description: Meer informatie over het beperken van toegang tot gegevens bewerkingen met Azure Cosmos DB
author: voellm
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/9/2019
ms.author: tvoellm
ms.openlocfilehash: 03cad9e4c3752b5f35be785a6280bf18aaa14860
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/10/2019
ms.locfileid: "74980372"
---
# <a name="restrict-user-access-to-data-operations-only"></a>Gebruikers toegang beperken tot alleen gegevens bewerkingen

In Azure Cosmos DB zijn er twee manieren om uw interacties te verifiëren met de database service:
- het gebruik van uw Azure Active Directory identiteit bij het communiceren met de Azure Portal,
- het gebruik van Azure Cosmos DB [sleutels](secure-access-to-data.md#master-keys) of [bron tokens](secure-access-to-data.md#resource-tokens) bij het uitgeven van aanroepen van api's en sdk's.

Elke verificatie methode biedt toegang tot verschillende sets bewerkingen, met enkele overlap pingen: ![splitsen van bewerkingen per verificatie type](./media/how-to-restrict-user-data/operations.png)

In sommige gevallen wilt u mogelijk bepaalde gebruikers van uw organisatie beperken tot het uitvoeren van gegevens bewerkingen (dat wil zeggen ruwe aanvragen en query's). Dit is meestal het geval voor ontwikkel aars die geen resources hoeven te maken of verwijderen, of de ingerichte door Voer van de containers waarmee ze werken, wijzigen.

U kunt de toegang beperken door de volgende stappen uit te voeren:
1. Het maken van een aangepaste Azure Active Directory rol voor de gebruikers waarvoor u de toegang wilt beperken. De aangepaste Active Directory rol moet een nauw keurig toegangs niveau hebben voor bewerkingen die gebruikmaken van de [gedetailleerde acties](../role-based-access-control/resource-provider-operations.md#microsoftdocumentdb)van Azure Cosmos db.
1. De uitvoering van niet-gegevens bewerkingen met sleutels niet toestaan. U kunt dit doen door deze bewerkingen te beperken tot alleen Azure Resource Manager-aanroepen.

In de volgende secties van dit artikel ziet u hoe u deze stappen uitvoert.

> [!NOTE]
> Als u de opdrachten in de volgende secties wilt uitvoeren, moet u Azure PowerShell module 3.0.0 of hoger installeren, evenals de [rol van Azure-eigenaar](../role-based-access-control/built-in-roles.md#owner) van het abonnement dat u wilt wijzigen.

Vervang in de Power shell-scripts in de volgende secties de volgende tijdelijke aanduidingen door de waarden die specifiek zijn voor uw omgeving:
- `$MySubscriptionId`: de abonnements-ID die het Azure Cosmos-account bevat waarvoor u de machtigingen wilt beperken. Bijvoorbeeld: `e5c8766a-eeb0-40e8-af56-0eb142ebf78e`.
- `$MyResourceGroupName`: de resource groep met het Azure Cosmos-account. Bijvoorbeeld: `myresourcegroup`.
- `$MyAzureCosmosDBAccountName`: de naam van uw Azure Cosmos-account. Bijvoorbeeld: `mycosmosdbsaccount`.
- `$MyUserName`: de aanmelding (username@domain) van de gebruiker voor wie u de toegang wilt beperken. Bijvoorbeeld: `cosmosdbuser@contoso.com`.

## <a name="select-your-azure-subscription"></a>selecteer uw Azure-abonnement

Voor Azure PowerShell opdrachten moet u zich aanmelden en het abonnement selecteren om de opdrachten uit te voeren:

```azurepowershell
Login-AzAccount
Select-AzSubscription $MySubscriptionId
```

## <a name="create-the-custom-azure-active-directory-role"></a>De aangepaste Azure Active Directory rol maken

Met het volgende script maakt u een Azure Active Directory roltoewijzing met ' alleen sleutel toegang voor Azure Cosmos-accounts. De rol is gebaseerd op [aangepaste rollen voor Azure-resources](../role-based-access-control/custom-roles.md) en [granulaire acties voor Azure Cosmos DB](../role-based-access-control/resource-provider-operations.md#microsoftdocumentdb). Deze rollen en acties maken deel uit van de `Microsoft.DocumentDB` Azure Active Directory naam ruimte.

1. Maak eerst een JSON-document met de naam `AzureCosmosKeyOnlyAccess.json` met de volgende inhoud:

    ```
    {
        "Name": "Azure Cosmos DB Key Only Access Custom Role",
        "Id": "00000000-0000-0000-0000-0000000000",
        "IsCustom": true,
        "Description": "This role restricts the user to read the account keys only.",
        "Actions":
        [
            "Microsoft.DocumentDB/databaseAccounts/listKeys/action"
        ],
        "NotActions": [],
        "DataActions": [],
        "NotDataActions": [],
        "AssignableScopes":
        [
            "/subscriptions/$MySubscriptionId"
        ]
    }
    ```

1. Voer de volgende opdrachten uit om de roltoewijzing te maken en toe te wijzen aan de gebruiker:

    ```azurepowershell
    New-AzRoleDefinition -InputFile "AzureCosmosKeyOnlyAccess.json"
    New-AzRoleAssignment -SignInName $MyUserName -RoleDefinitionName "Azure Cosmos DB Key Only Access Custom Role" -ResourceGroupName $MyResourceGroupName -ResourceName $MyAzureCosmosDBAccountName -ResourceType "Microsoft.DocumentDb/databaseAccounts"
    ```

## <a name="disallow-the-execution-of-non-data-operations"></a>Uitvoering van niet-gegevens bewerkingen niet toestaan

Met de volgende opdrachten verwijdert u de mogelijkheid om sleutels te gebruiken voor:
- resources maken, wijzigen of verwijderen
- container instellingen bijwerken (inclusief indexerings beleid, door Voer enz.).

```azurepowershell
$cdba = Get-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName $MyResourceGroupName -ResourceName $MyAzureCosmosDBAccountName
$cdba.Properties.disableKeyBasedMetadataWriteAccess="True"
$cdba | Set-AzResource -Force
```

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [toegangs beheer op basis van rollen van Cosmos DB](role-based-access-control.md)
- Bekijk een overzicht van [beveiligde toegang tot gegevens in Cosmos DB](secure-access-to-data.md)
