---
title: Voor beeld van Azure CLI-script-een Azure-app configuratie archief verwijderen
titleSuffix: Azure App Configuration
description: Azure CLI-voorbeeldscript - een Azure App Configuration-archief verwijderen
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: balans
editor: ''
ms.service: azure-app-configuration
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: azure-app-configuration
ms.date: 02/24/2019
ms.author: yegu
ms.custom: mvc
ms.openlocfilehash: d5a80288fcd5b0216a9bf3ca322203f672f381d0
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/25/2019
ms.locfileid: "75413379"
---
# <a name="delete-an-azure-app-configuration-store"></a>Een Azure App Configuration-archief verwijderen

Met dit voorbeeldscript verwijdert u een exemplaar van Azure App Configuration.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor dit artikel gebruikmaken van Azure CLI versie 2.0 of hoger. Voer `az --version` uit om de versie te bekijken. Als u uw CLI wilt installeren of upgraden, raadpleegt u [De Azure CLI installeren](/cli/azure/install-azure-cli).

U moet eerst de CLI-extensie voor het Azure App Configuration-archief installeren door de volgende opdracht uit te voeren:

        az extension add -n appconfig

## <a name="sample-script"></a>Voorbeeldscript

```azurecli-interactive
#/bin/bash

# Delete an App Configuration store named myTestAppConfigStore from the Resource Group myResourceGroup
az appconfig delete --name myTestAppConfigStore --resource-group myResourceGroup
```

[!INCLUDE [cli-script-cleanup](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Uitleg van het script

In dit script worden de volgende opdrachten gebruikt om een app-configuratie archief te verwijderen. Elke opdracht in de tabel is gekoppeld aan de specifieke documentatie over de opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [az appconfig kv delete](/cli/azure/ext/appconfig/appconfig#ext-appconfig-az-appconfig-delete) | Hiermee verwijdert u een bron voor het configuratie archief van de app. |

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de [documentatie van Azure CLI](/cli/azure) voor meer informatie over de Azure CLI.

Extra CLI-script voorbeelden voor configuratie van apps vindt u in de voor beelden van de [Azure-app configuratie-cli](../cli-samples.md).
