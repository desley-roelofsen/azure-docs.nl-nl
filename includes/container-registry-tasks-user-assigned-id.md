---
title: bestand opnemen
description: bestand opnemen
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: include
ms.date: 07/12/2019
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 0e2acb346fad87e0c1c7fd7de1389d8fc86206d0
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/30/2019
ms.locfileid: "68642109"
---
### <a name="create-a-user-assigned-identity"></a>Een door de gebruiker toegewezen identiteit maken

Maak een identiteit met de naam *myACRTasksId* in uw abonnement met behulp van de opdracht [AZ Identity Create][az-identity-create] . U kunt dezelfde resource groep gebruiken die u eerder hebt gebruikt voor het maken van een container register of een andere.

```azurecli-interactive
az identity create --resource-group myResourceGroup --name myACRTasksId
```

Als u de door de gebruiker toegewezen identiteit in de volgende stappen wilt configureren, gebruikt u de opdracht [AZ Identity show][az-identity-show] om de resource-id van de identiteit op te slaan, Service-Principal-id en client-id in variabelen.

```azurecli
# Get resource ID of the user-assigned identity
resourceID=$(az identity show --resource-group myResourceGroup --name myACRTasksId --query id --output tsv)

# Get service principal ID of the user-assigned identity
principalID=$(az identity show --resource-group myResourceGroup --name myACRTasksId --query principalId --output tsv)

# Get client ID of the user-assigned identity
clientID=$(az identity show --resource-group myResourceGroup --name myACRTasksId --query clientId --output tsv)
```

<!-- LINKS - Internal -->
[az-identity-create]: /cli/azure/identity#az-identity-create
[az-identity-show]: /cli/azure/identity#az-identity-show