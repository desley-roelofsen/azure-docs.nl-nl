---
title: Door de gebruiker toegewezen beheerde identiteiten beheren met REST-Azure AD
description: Stapsgewijze instructies voor het maken, weer geven en verwijderen van een door de gebruiker toegewezen beheerde identiteit om REST API aanroepen te maken.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/26/2018
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: 39e108451e4c19e77e01b5bcc5d8dd21e86ad73a
ms.sourcegitcommit: a678f00c020f50efa9178392cd0f1ac34a86b767
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/26/2019
ms.locfileid: "74547418"
---
# <a name="create-list-or-delete-a-user-assigned-managed-identity-using-rest-api-calls"></a>Een door de gebruiker toegewezen beheerde identiteit maken, weer geven of verwijderen met REST API-aanroepen

[!INCLUDE [preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Beheerde identiteiten voor Azure-resources biedt Azure-Services de mogelijkheid om te verifiëren bij services die ondersteuning bieden voor Azure AD-verificatie, zonder dat er referenties in uw code nodig zijn. 

In dit artikel leert u hoe u een door de gebruiker toegewezen beheerde identiteit kunt maken, weer geven en verwijderen met behulp van krul om REST API aanroepen te maken.

## <a name="prerequisites"></a>Vereisten

- Als u niet bekend bent met beheerde identiteiten voor Azure-resources, raadpleegt u de [sectie Overzicht](overview.md). **Controleer het [verschil tussen een door het systeem toegewezen en door de gebruiker toegewezen beheerde identiteit](overview.md#how-does-the-managed-identities-for-azure-resources-work)** .
- Als u nog geen Azure-account hebt, [registreer u dan voor een gratis account](https://azure.microsoft.com/free/) voordat u verdergaat.
- Als u Windows gebruikt, installeert u het [Windows-subsysteem voor Linux](https://msdn.microsoft.com/commandline/wsl/about) of gebruikt u de [Azure Cloud Shell](../../cloud-shell/overview.md) in de Azure Portal.
- Als u het [Windows-subsysteem voor Linux](https://msdn.microsoft.com/commandline/wsl/about) of een [Linux-distributie besturingssysteem](/cli/azure/install-azure-cli-apt?view=azure-cli-latest)gebruikt, installeert u [de lokale Azure cli-console](/cli/azure/install-azure-cli).
- Als u de lokale Azure CLI-console gebruikt, meldt u zich aan bij Azure met `az login` met een account dat is gekoppeld aan het Azure-abonnement waarvoor u door de gebruiker toegewezen beheerde identiteits gegevens wilt implementeren of ophalen.
- Haal een Bearer-toegangs token op met behulp van `az account get-access-token` om de volgende door de gebruiker toegewezen beheerde identiteits bewerkingen uit te voeren.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="create-a-user-assigned-managed-identity"></a>Een door de gebruiker toegewezen beheerde identiteit maken 

Als u een door de gebruiker toegewezen beheerde identiteit wilt maken, moet uw account beschikken over de rol toewijzing [beheerde identiteits bijdrage](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) .

[!INCLUDE [ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]

```bash
curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroup
s/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>?api-version=2015-08-31-preview' -X PUT -d '{"loc
ation": "<LOCATION>"}' -H "Content-Type: application/json" -H "Authorization: Bearer <ACCESS TOKEN>"
```

```HTTP
PUT https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroup
s/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>?api-version=2015-08-31-preview HTTP/1.1
```

**Aanvraag headers**

|Aanvraagheader  |Beschrijving  |
|---------|---------|
|*Inhouds type*     | Vereist. Ingesteld op `application/json`.        |
|*Autorisatie*     | Vereist. Stel in op een geldig `Bearer` toegangs token.        |

**Aanvraag tekst**

|Naam  |Beschrijving  |
|---------|---------|
|location     | Vereist. Resource locatie.        |

## <a name="list-user-assigned-managed-identities"></a>Door de gebruiker toegewezen beheerde identiteiten weer geven

Als u een door de gebruiker toegewezen beheerde identiteit wilt weer geven/lezen, moet uw account beschikken over de rol [Managed Identity](/azure/role-based-access-control/built-in-roles#managed-identity-operator) of [Managed id contributor](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) .

```bash
curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities?api-version=2015-08-31-preview' -H "Authorization: Bearer <ACCESS TOKEN>"
```

```HTTP
GET https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities?api-version=2015-08-31-preview HTTP/1.1
```

|Aanvraagheader  |Beschrijving  |
|---------|---------|
|*Inhouds type*     | Vereist. Ingesteld op `application/json`.        |
|*Autorisatie*     | Vereist. Stel in op een geldig `Bearer` toegangs token.        |

## <a name="delete-a-user-assigned-managed-identity"></a>Een door de gebruiker toegewezen beheerde identiteit verwijderen

Als u een door de gebruiker toegewezen beheerde identiteit wilt verwijderen, moet uw account beschikken over de rol toewijzing [beheerde identiteits bijdrage](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) .

> [!NOTE]
> Als u een door de gebruiker toegewezen beheerde identiteit verwijdert, wordt de verwijzing niet verwijderd uit een resource waaraan deze is toegewezen. Een door de gebruiker toegewezen beheerde identiteit verwijderen uit een virtuele machine met behulp van krul Zie [een door de gebruiker toegewezen identiteit verwijderen uit een Azure-VM](qs-configure-rest-vm.md#remove-a-user-assigned identity-from-an-azure-vm).

```bash
curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroup
s/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>?api-version=2015-08-31-preview' -X DELETE -H "Authorization: Bearer <ACCESS TOKEN>"
```

```HTTP
DELETE https://management.azure.com/subscriptions/80c696ff-5efa-4909-a64d-f1b616f423ca/resourceGroups/TestRG/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>?api-version=2015-08-31-preview HTTP/1.1
```
|Aanvraagheader  |Beschrijving  |
|---------|---------|
|*Inhouds type*     | Vereist. Ingesteld op `application/json`.        |
|*Autorisatie*     | Vereist. Stel in op een geldig `Bearer` toegangs token.        |

## <a name="next-steps"></a>Volgende stappen

Voor informatie over het toewijzen van een door de gebruiker toegewezen beheerde identiteit aan een Azure VM-VMSS met behulp van krul, [configureert u beheerde identiteiten voor Azure-resources op een Azure-VM met behulp van rest API aanroepen](qs-configure-rest-vm.md#user-assigned-managed-identity) en [Configureer beheerde identiteiten voor Azure-resources op een schaalset voor virtuele machines met behulp van rest API-aanroepen](qs-configure-rest-vmss.md#user-assigned-managed-identity).
