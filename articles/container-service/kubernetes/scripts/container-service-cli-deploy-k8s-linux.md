---
title: Voorbeeld van Azure CLI-script - ACS Linux Kubernetes-cluster maken | Microsoft Docs
description: Voorbeeld van Azure CLI-script - ACS Linux Kubernetes-cluster maken
services: container-service
documentationcenter: ''
author: iainfoulds
manager: jeconnoc
editor: ''
tags: acs, azure-container-service
keywords: Docker, Containers, Micro-services, Kubernetes, DC/OS, Azure
ms.assetid: ''
ms.service: container-service
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/30/2017
ms.author: iainfou
ms.openlocfilehash: 11549e730e38c72e09ed2e94a72113c6e7e5bbb9
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60901826"
---
# <a name="deprecated-create-an-azure-container-service-kubernetes-linux-cluster"></a>(AFGESCHAFT) Een Kubernetes Linux-cluster maken in Azure Container Service

[!INCLUDE [ACS deprecation](../../../../includes/container-service-kubernetes-deprecation.md)]

In dit voorbeeld wordt een cluster van Azure Container Service gemaakt waarin containers op basis van Kubernetes voor Linux worden uitgevoerd.

[!INCLUDE [sample-cli-install](../../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Voorbeeldscript

```azurecli
az group create --name myResourceGroup --location eastus

az acs create \
  --orchestrator-type kubernetes \
  --resource-group myResourceGroup \
  --name myK8SCluster \
  --generate-ssh-keys
```

## <a name="clean-up-deployment"></a>Opschonen van implementatie 

Gebruik de volgende opdracht om de resourcegroep, VM, en alle gerelateerde resources te verwijderen.

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Uitleg van het script

In dit script worden de volgende opdrachten gebruikt om de implementatie te maken. Elk item in de tabel is een koppeling naar specifieke documentatie over de opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#az-group-create) | Hiermee wordt een resourcegroep gemaakt waarin alle resources worden opgeslagen. |
| [az acs create](https://docs.microsoft.com/cli/azure/acs#az-acs-create) | Hiermee maakt u een ACS-cluster. |

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de [documentatie van Azure CLI](https://docs.microsoft.com/cli/azure) voor meer informatie over de Azure CLI.

Meer Azure Container Service CLI-voorbeeldscripts vindt u in de [documentatie van Azure Container Service](../cli-samples.md).

