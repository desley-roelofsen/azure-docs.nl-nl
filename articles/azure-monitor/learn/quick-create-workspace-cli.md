---
title: Een Log Analytics-werk ruimte maken met behulp van Azure CLI | Microsoft Docs
description: Meer informatie over het maken van een Log Analytics-werk ruimte voor het inschakelen van beheer oplossingen en gegevens verzameling vanuit uw Cloud en on-premises omgevingen met Azure CLI.
ms.service: azure-monitor
ms.subservice: logs
ms.topic: conceptual
author: mgoedtel
ms.author: magoedte
ms.date: 03/12/2019
ms.openlocfilehash: 18bfc99ded6e3e9171fbb20fbf329700817829de
ms.sourcegitcommit: 5acd8f33a5adce3f5ded20dff2a7a48a07be8672
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/24/2019
ms.locfileid: "72894998"
---
# <a name="create-a-log-analytics-workspace-with-azure-cli-20"></a>Een Log Analytics-werk ruimte maken met Azure CLI 2,0

De Azure CLI 2.0 wordt gebruikt voor het maken en beheren van Azure-resources vanaf de opdrachtregel of in scripts. In deze Quick start ziet u hoe u met Azure CLI 2,0 een Log Analytics-werk ruimte implementeert in Azure Monitor. Een Log Analytics-werk ruimte is een unieke omgeving voor Azure Monitor logboek gegevens. Elke werk ruimte heeft een eigen gegevens opslagplaats en-configuratie, en gegevens bronnen en-oplossingen zijn geconfigureerd om hun gegevens op te slaan in een bepaalde werk ruimte. U hebt een Log Analytics-werk ruimte nodig als u van plan bent om gegevens te verzamelen uit de volgende bronnen:

* Azure-resources in uw abonnement  
* On-premises computers die worden bewaakt door System Center Operations Manager  
* Verzamelingen van apparaten uit System Center Configuration Manager  
* Diagnose- of logboekgegevens van Azure Storage  
 
Zie de volgende onderwerpen voor andere bronnen, zoals virtuele Azure-machines en Windows-of Linux-Vm's in uw omgeving:

* [Gegevens verzamelen van virtuele machines van Azure](../learn/quick-collect-azurevm.md)
* [Gegevens verzamelen van een hybride Linux-computer](../learn/quick-collect-linux-computer.md)
* [Gegevens verzamelen van de hybride Windows-computer](quick-collect-windows-computer.md)

Als u nog geen abonnement op Azure hebt, maak dan [een gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor deze snelstartgids de Azure CLI versie 2.0.30 of hoger uitvoeren. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

## <a name="create-a-workspace"></a>Een werkruimte maken
Maak een werk ruimte met [AZ Group Deployment Create](https://docs.microsoft.com/cli/azure/group/deployment?view=azure-cli-latest#az-group-deployment-create). In het volgende voor beeld wordt een werk ruimte op de locatie *ooster* gemaakt met behulp van een resource manager-sjabloon van uw lokale machine. De JSON-sjabloon is zo geconfigureerd dat alleen u wordt gevraagd om de naam van de werk ruimte en geeft een standaard waarde op voor de andere para meters die waarschijnlijk worden gebruikt als een standaard configuratie in uw omgeving. Of u kunt de sjabloon opslaan in een Azure-opslag account voor gedeelde toegang in uw organisatie. Zie [resources implementeren met Resource Manager-sjablonen en Azure cli](../../azure-resource-manager/resource-group-template-deploy-cli.md) voor meer informatie over het werken met sjablonen.

Zie [regio's log Analytics is beschikbaar in](https://azure.microsoft.com/regions/services/) en zoek naar Azure monitor in het veld **zoeken naar een product** voor meer informatie over regio's die worden ondersteund. 

Met de volgende para meters wordt een standaard waarde ingesteld:

* locatie-standaard naar VS-Oost
* SKU: wordt standaard ingesteld op de nieuwe prijs categorie per GB die is uitgebracht in het prijs model van april 2018

>[!WARNING]
>Als u een Log Analytics-werk ruimte maakt of configureert in een abonnement dat is aangemeld met het nieuwe prijs model van april 2018, is de enige geldige Log Analytics prijs categorie **PerGB2018**. 
>

### <a name="create-and-deploy-template"></a>Sjabloon maken en implementeren

1. Kopieer en plak de volgende JSON-syntaxis in het bestand:

    ```json
    {
    "$schema": "https://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "workspaceName": {
            "type": "String",
            "metadata": {
              "description": "Specifies the name of the workspace."
            }
        },
        "location": {
            "type": "String",
            "allowedValues": [
              "eastus",
              "westus"
            ],
            "defaultValue": "eastus",
            "metadata": {
              "description": "Specifies the location in which to create the workspace."
            }
        },
        "sku": {
            "type": "String",
            "allowedValues": [
              "Standalone",
              "PerNode",
              "PerGB2018"
            ],
            "defaultValue": "PerGB2018",
            "metadata": {
            "description": "Specifies the service tier of the workspace: Standalone, PerNode, Per-GB"
        }
          }
    },
    "resources": [
        {
            "type": "Microsoft.OperationalInsights/workspaces",
            "name": "[parameters('workspaceName')]",
            "apiVersion": "2015-11-01-preview",
            "location": "[parameters('location')]",
            "properties": {
                "sku": {
                    "Name": "[parameters('sku')]"
                },
                "features": {
                    "searchVersion": 1
                }
            }
          }
       ]
    }
    ```

2. Bewerk de sjabloon om te voldoen aan uw vereisten. Raadpleeg de naslag informatie over [micro soft. OperationalInsights/werkruimte sjablonen](https://docs.microsoft.com/azure/templates/microsoft.operationalinsights/workspaces) als u wilt weten welke eigenschappen en waarden worden ondersteund. 
3. Sla dit bestand op als **deploylaworkspacetemplate. json** naar een lokale map.   
4. U kunt deze sjabloon nu implementeren. Gebruik de volgende opdrachten in de map met de sjabloon. Wanneer u wordt gevraagd om de naam van een werk ruimte, geeft u een naam op die wereld wijd uniek is voor alle Azure-abonnementen.

    ```azurecli
    az group deployment create --resource-group <my-resource-group> --name <my-deployment-name> --template-file deploylaworkspacetemplate.json
    ```

De implementatie kan enkele minuten duren. Wanneer de bewerking is voltooid, ziet u een bericht dat lijkt op het volgende:

![Voor beeld van resultaat wanneer de implementatie is voltooid](media/quick-create-workspace-cli/template-output-01.png)

## <a name="next-steps"></a>Volgende stappen
Nu u een werk ruimte beschikbaar hebt, kunt u een verzameling van de telemetrie-bewaking configureren, Zoek opdrachten in Logboeken uitvoeren om die gegevens te analyseren en een beheer oplossing toevoegen om extra gegevens en analytische inzichten te leveren.  

* Zie [Azure service-logboeken en-metrische gegevens verzamelen voor gebruik in log Analytics](../platform/collect-azure-metrics-logs.md)voor informatie over het inschakelen van het verzamelen van data van Azure-resources met Azure Diagnostics of Azure Storage.  
* Voeg [System Center Operations Manager als gegevens bron](../platform/om-agents.md) toe om gegevens te verzamelen van agents die uw Operations Manager beheer groep rapporteren en op te slaan in uw log Analytics-werk ruimte.  
* Verbind [Configuration Manager](../platform/collect-sccm.md) om computers te importeren die lid zijn van verzamelingen in de hiërarchie.  
* Bekijk de beschik bare [bewakings oplossingen](../insights/solutions.md) en hoe u een oplossing kunt toevoegen aan of verwijderen uit uw werk ruimte.
