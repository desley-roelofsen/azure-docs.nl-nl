---
title: Snelstartgids-Ansible playbooks uitvoeren via bash in Azure Cloud Shell
description: In deze Quick Start leert u hoe u verschillende Ansible-taken kunt uitvoeren met bash in Azure Cloud Shell
keywords: ansible, azure, devops, bash, cloudshell, playbook, bash
ms.topic: quickstart
ms.date: 04/30/2019
ms.openlocfilehash: d04708be82a704c2ce20a928380fca1d325493da
ms.sourcegitcommit: 28688c6ec606ddb7ae97f4d0ac0ec8e0cd622889
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/18/2019
ms.locfileid: "74155974"
---
# <a name="quickstart-run-ansible-playbooks-via-bash-in-azure-cloud-shell"></a>Snelstartgids: Ansible playbooks uitvoeren via bash in Azure Cloud Shell

Azure Cloud Shell is een interactieve, voor browsers toegankelijke shell voor het beheer van Azure-resources. Cloud Shell biedt u de mogelijkheid om een bash-of Power shell-opdracht regel te gebruiken. In dit artikel gebruikt u bash binnen Azure Cloud Shell om een Ansible Playbook uit te voeren.

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../../includes/open-source-devops-prereqs-azure-subscription.md)]
- **Azure Cloud shell configureren** : Zie [Quick start voor bash in azure Cloud shell](https://docs.microsoft.com/azure/cloud-shell/quickstart)als u geen ervaring hebt met Azure Cloud shell.
[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="automatic-credential-configuration"></a>Automatische configuratie van referenties

Ansible wordt geverifieerd bij Azure wanneer u zich aanmeldt bij Cloud Shell om de infrastructuur te beheren zonder extra configuratie. 

Wanneer u met meerdere abonnementen werkt, geeft u de Ansible op die moet worden gebruikt door de omgevings variabele `AZURE_SUBSCRIPTION_ID` te exporteren. 

Voer de volgende opdracht uit om al uw Azure-abonnementen weer te geven:

```azurecli-interactive
az account list
```

Als u uw Azure-abonnements-ID gebruikt, stelt u de `AZURE_SUBSCRIPTION_ID` als volgt in:

```azurecli-interactive
export AZURE_SUBSCRIPTION_ID=<your-subscription-id>
```

## <a name="verify-the-configuration"></a>De configuratie controleren
Gebruik Ansible om een Azure-resource groep te maken om de geslaagde configuratie te controleren.

[!INCLUDE [create-resource-group-with-ansible.md](../../includes/ansible-snippet-create-resource-group.md)]

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"] 
> [Snelstartgids: virtuele machine in azure configureren met behulp van Ansible](/azure/virtual-machines/linux/ansible-create-vm)