---
title: Een AKS-cluster (Azure Kubernetes service) bewaken dat is geïmplementeerd | Microsoft Docs
description: Meer informatie over het inschakelen van bewaking van een Azure Kubernetes service-cluster (AKS) met Azure Monitor voor containers die al zijn geïmplementeerd in uw abonnement.
ms.topic: conceptual
ms.date: 09/12/2019
ms.openlocfilehash: eced371f7d44b486d671c2c22ca9fbb4c0b65fbb
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/25/2019
ms.locfileid: "75405493"
---
# <a name="enable-monitoring-of-azure-kubernetes-service-aks-cluster-already-deployed"></a>Bewaking van het cluster van Azure Kubernetes service (AKS) inschakelen dat al is geïmplementeerd

In dit artikel wordt beschreven hoe u Azure Monitor instelt voor containers voor het bewaken van beheerde Kubernetes-clusters die worden gehost op de [Azure Kubernetes-service](https://docs.microsoft.com/azure/aks/) die al is geïmplementeerd in uw abonnement.

U kunt de bewaking van een AKS-cluster inschakelen dat al is geïmplementeerd met een van de ondersteunde methoden:

* Azure-CLI
* Terraform
* [Vanuit Azure monitor](#enable-from-azure-monitor-in-the-portal) of [rechtstreeks vanuit het AKS-cluster](#enable-directly-from-aks-cluster-in-the-portal) in de Azure Portal 
* Met de [meegeleverde Azure Resource Manager sjabloon](#enable-using-an-azure-resource-manager-template) met behulp van de Azure PowerShell-cmdlet `New-AzResourceGroupDeployment` of met Azure cli. 

## <a name="sign-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

Meld u aan bij de [Azure Portal](https://portal.azure.com). 

## <a name="enable-using-azure-cli"></a>Inschakelen met behulp van Azure CLI

De volgende stap kunt bewaking van uw AKS-cluster met behulp van Azure CLI. In dit voorbeeld zijn u niet verplicht per maken of geef een bestaande werkruimte. Met deze opdracht vereenvoudigt het proces voor u door het maken van een standaard-werkruimte in de standaard-resourcegroep van het AKS-cluster-abonnement als deze niet al in de regio bestaat.  De standaardwerk ruimte die wordt gemaakt, is vergelijkbaar met de indeling van de *DefaultWorkspace-\<GUID >\<regio >* .  

```azurecli
az aks enable-addons -a monitoring -n MyExistingManagedCluster -g MyExistingManagedClusterRG  
```

De uitvoer ziet eruit als in het volgende:

```azurecli
provisioningState       : Succeeded
```

### <a name="integrate-with-an-existing-workspace"></a>Integreren met een bestaande werk ruimte

Als u liever met een bestaande werk ruimte zou integreren, voert u de volgende stappen uit om eerst de volledige Resource-ID te identificeren van uw Log Analytics werk ruimte die is vereist voor de `--workspace-resource-id`-para meter, en voert u vervolgens de opdracht uit om de invoeg toepassing bewaking met de opgegeven werk ruimte in te scha kelen.  

1. Een lijst met alle abonnementen waartoe u toegang hebt met behulp van de volgende opdracht:

    ```azurecli
    az account list --all -o table
    ```

    De uitvoer ziet eruit als in het volgende:

    ```azurecli
    Name                                  CloudName    SubscriptionId                        State    IsDefault
    ------------------------------------  -----------  ------------------------------------  -------  -----------
    Microsoft Azure                       AzureCloud   68627f8c-91fO-4905-z48q-b032a81f8vy0  Enabled  True
    ```

    Kopieer de waarde voor **SubscriptionId**.

2. Schakel over naar het abonnement dat als host fungeert voor de Log Analytics-werk ruimte met behulp van de volgende opdracht:

    ```azurecli
    az account set -s <subscriptionId of the workspace>
    ```

3. In het volgende voor beeld wordt de lijst met werk ruimten in uw abonnementen weer gegeven in de standaard JSON-indeling. 

    ```
    az resource list --resource-type Microsoft.OperationalInsights/workspaces -o json
    ```

    Zoek in de uitvoer de naam van de werk ruimte en kopieer de volledige Resource-ID van die Log Analytics werk ruimte onder de veld **-id**.
 
4. Voer de volgende opdracht uit om de invoeg toepassing bewaking in te scha kelen, waarbij u de waarde voor de para meter `--workspace-resource-id` vervangt. De teken reeks waarde moet binnen de dubbele aanhalings tekens staan:

    ```azurecli
    az aks enable-addons -a monitoring -n ExistingManagedCluster -g ExistingManagedClusterRG --workspace-resource-id "/subscriptions/<SubscriptionId>/resourceGroups/<ResourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<WorkspaceName>"
    ```

    De uitvoer ziet eruit als in het volgende:

    ```azurecli
    provisioningState       : Succeeded
    ```

## <a name="enable-using-terraform"></a>Inschakelen met behulp van Terraform

1. Voeg de **oms_agent** invoegtoepassing profiel aan de bestaande [azurerm_kubernetes_cluster resource](https://www.terraform.io/docs/providers/azurerm/d/kubernetes_cluster.html#addon_profile)

   ```
   addon_profile {
    oms_agent {
      enabled                    = true
      log_analytics_workspace_id = "${azurerm_log_analytics_workspace.test.id}"
     }
   }
   ```

2. Voeg de [azurerm_log_analytics_solution](https://www.terraform.io/docs/providers/azurerm/r/log_analytics_solution.html) de stappen in de documentatie van Terraform te volgen.

## <a name="enable-from-azure-monitor-in-the-portal"></a>Inschakelen van Azure Monitor in de portal 

Als u wilt inschakelen voor bewaking van uw AKS-cluster in Azure portal van Azure Monitor, het volgende doen:

1. Selecteer in de Azure portal, **Monitor**. 

2. Selecteer **Containers** in de lijst.

3. Op de **Monitor - containers** weergeeft, schakelt **clusters niet bewaakt**.

4. In de lijst met clusters niet-bewaakt, de container niet vinden in de lijst en klik op **inschakelen**.   

5. Op de **Onboarding naar Azure Monitor voor containers** pagina, hebt u een bestaande Log Analytics-werkruimte in hetzelfde abonnement bevinden als het cluster, selecteert u deze in de vervolgkeuzelijst.  
    De lijst worden er de standaardwerkruimte en de locatie die het AKS-container is geïmplementeerd op in het abonnement. 

    ![AKS-Container inzichten, bewaking inschakelen](./media/container-insights-onboard/kubernetes-onboard-brownfield-01.png)

    >[!NOTE]
    >Als u maken van een nieuwe werkruimte voor logboekanalyse wilt voor het opslaan van de gegevens uit het cluster, volg de instructies in [een Log Analytics-werkruimte maken](../../azure-monitor/learn/quick-create-workspace.md). Zorg ervoor dat de werkruimte maakt in hetzelfde abonnement dat voor de AKS-container is geïmplementeerd. 
 
Wanneer u bewaking inschakelt, is het duurt ongeveer 15 minuten voordat u de gezondheid van metrische gegevens voor het cluster kunt weergeven. 

## <a name="enable-directly-from-aks-cluster-in-the-portal"></a>Rechtstreeks inschakelen vanuit het AKS-cluster in de portal

Ga als volgt te werk om bewaking rechtstreeks vanuit een van uw AKS-clusters in de Azure Portal in te scha kelen:

1. Selecteer in de Azure-portal de optie **Alle services**. 

2. Begin met typen in de lijst met resources **Containers**.  De lijst gefilterd op basis van uw invoer. 

3. Selecteer **Kubernetes-services**.  

    ![De koppeling van Kubernetes-services](./media/container-insights-onboard/portal-search-containers-01.png)

4. Selecteer een container in de lijst met containers.

5. Selecteer op de overzichtspagina van container **Containers bewaken**.  

6. Op de **Onboarding naar Azure Monitor voor containers** pagina, hebt u een bestaande Log Analytics-werkruimte in hetzelfde abonnement bevinden als het cluster, selecteert u deze in de vervolgkeuzelijst.  
    De lijst worden er de standaardwerkruimte en de locatie die het AKS-container is geïmplementeerd op in het abonnement. 

    ![Statuscontrole van AKS container inschakelen](./media/container-insights-onboard/kubernetes-onboard-brownfield-02.png)

    >[!NOTE]
    >Als u maken van een nieuwe werkruimte voor logboekanalyse wilt voor het opslaan van de gegevens uit het cluster, volg de instructies in [een Log Analytics-werkruimte maken](../../azure-monitor/learn/quick-create-workspace.md). Zorg ervoor dat de werkruimte maakt in hetzelfde abonnement dat voor de AKS-container is geïmplementeerd. 
 
Wanneer u bewaking inschakelt, is het duurt ongeveer 15 minuten voordat u de operationele gegevens voor het cluster kunt weergeven. 

## <a name="enable-using-an-azure-resource-manager-template"></a>Inschakelen met behulp van een Azure Resource Manager sjabloon

Deze methode omvat twee JSON-sjablonen. Een sjabloon Hiermee geeft u de configuratie voor bewaking en de andere bevat parameterwaarden die u configureert voor het volgende opgeven:

* De AKS container resource-ID. 
* De resourcegroep die in het cluster is geïmplementeerd.

>[!NOTE]
>De sjabloon opnieuw moet worden geïmplementeerd in dezelfde resourcegroep bevinden als het cluster.
>

De Log Analytics-werk ruimte moet worden gemaakt voordat u bewaking met behulp van Azure PowerShell of CLI inschakelt. Voor het maken van de werkruimte, u kunt dit instellen via [Azure Resource Manager](../../azure-monitor/platform/template-workspace-configuration.md), tot en met [PowerShell](../scripts/powershell-sample-create-workspace.md?toc=%2fpowershell%2fmodule%2ftoc.json), of in de [Azure-portal](../../azure-monitor/learn/quick-create-workspace.md).

Als u niet bekend met het concept bent van het implementeren van resources met behulp van een sjabloon, Zie:

* [Resources implementeren met Resource Manager-sjablonen en Azure PowerShell](../../azure-resource-manager/resource-group-template-deploy.md)

* [Resources implementeren met Resource Manager-sjablonen en Azure CLI](../../azure-resource-manager/resource-group-template-deploy-cli.md)

Als u ervoor de Azure CLI gebruiken kiest, moet u eerst installeren en de CLI lokaal gebruikt. U moet de Azure CLI-versie 2.0.59 of hoger uitvoeren. Voor het identificeren van uw versie uitvoeren `az --version`. Als u wilt installeren of upgraden van de Azure CLI, Zie [Azure CLI installeren](https://docs.microsoft.com/cli/azure/install-azure-cli). 

### <a name="create-and-execute-a-template"></a>Maken en uitvoeren van een sjabloon

1. Kopieer en plak de volgende JSON-syntaxis in het bestand:

    ```json
    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "aksResourceId": {
          "type": "string",
          "metadata": {
            "description": "AKS Cluster Resource ID"
          }
        },
        "aksResourceLocation": {
          "type": "string",
          "metadata": {
            "description": "Location of the AKS resource e.g. \"East US\""
          }
        },
        "aksResourceTagValues": {
          "type": "object",
          "metadata": {
            "description": "Existing all tags on AKS Cluster Resource"
          }
        },
        "workspaceResourceId": {
          "type": "string",
          "metadata": {
            "description": "Azure Monitor Log Analytics Resource ID"
          }
        }
      },
      "resources": [
        {
          "name": "[split(parameters('aksResourceId'),'/')[8]]",
          "type": "Microsoft.ContainerService/managedClusters",
          "location": "[parameters('aksResourceLocation')]",
          "tags": "[parameters('aksResourceTagValues')]",
          "apiVersion": "2018-03-31",
          "properties": {
            "mode": "Incremental",
            "id": "[parameters('aksResourceId')]",
            "addonProfiles": {
              "omsagent": {
                "enabled": true,
                "config": {
                  "logAnalyticsWorkspaceResourceID": "[parameters('workspaceResourceId')]"
                }
              }
            }
          }
        }
      ]
    }
    ```

2. Sla dit bestand als **existingClusterOnboarding.json** naar een lokale map.

3. Plak de volgende JSON-syntaxis in het bestand:

    ```json
    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "aksResourceId": {
          "value": "/subscriptions/<SubscriptionId>/resourcegroups/<ResourceGroup>/providers/Microsoft.ContainerService/managedClusters/<ResourceName>"
        },
        "aksResourceLocation": {
          "value": "<aksClusterLocation>"
        },
        "workspaceResourceId": {
          "value": "/subscriptions/<SubscriptionId>/resourceGroups/<ResourceGroup>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>"
        },
        "aksResourceTagValues": {
          "value": {
            "<existing-tag-name1>": "<existing-tag-value1>",
            "<existing-tag-name2>": "<existing-tag-value2>",
            "<existing-tag-nameN>": "<existing-tag-valueN>"
          }
        }
      }
    }
    ```

4. Bewerk de waarden voor **aksResourceId** en **aksResourceLocation** met behulp van de waarden op de **overzichts** pagina van AKS voor het AKS-cluster. De waarde voor **workspaceResourceId** is de volledige resource-ID van uw Log Analytics-werkruimte, waaronder de naam van de werkruimte. 

    Bewerk de waarden voor **aksResourceTagValues** zodat deze overeenkomen met de bestaande label waarden die zijn opgegeven voor het AKS-cluster.

5. Sla dit bestand als **existingClusterParam.json** naar een lokale map.

6. U kunt deze sjabloon nu implementeren. 

   * Als u wilt implementeren met Azure PowerShell, gebruikt u de volgende opdrachten in de map met de sjabloon:

       ```powershell
       New-AzResourceGroupDeployment -Name OnboardCluster -ResourceGroupName <ResourceGroupName> -TemplateFile .\existingClusterOnboarding.json -TemplateParameterFile .\existingClusterParam.json
       ```
       
       Wijzigen van de configuratie kan een paar minuten duren. Wanneer deze voltooid, wordt er een bericht weergegeven dat vergelijkbaar is met het volgende en het resultaat bevat:

       ```powershell
       provisioningState       : Succeeded
       ```

   * Als u wilt implementeren met Azure CLI, voert u de volgende opdrachten uit:
    
       ```azurecli
       az login
       az account set --subscription "Subscription Name"
       az group deployment create --resource-group <ResourceGroupName> --template-file ./existingClusterOnboarding.json --parameters @./existingClusterParam.json
       ```

       Wijzigen van de configuratie kan een paar minuten duren. Wanneer deze voltooid, wordt er een bericht weergegeven dat vergelijkbaar is met het volgende en het resultaat bevat:

       ```azurecli
       provisioningState       : Succeeded
       ```
     
       Wanneer u bewaking inschakelt, is het duurt ongeveer 15 minuten voordat u de gezondheid van metrische gegevens voor het cluster kunt weergeven. 

## <a name="verify-agent-and-solution-deployment"></a>Controleer of de implementatie van agent en de oplossing

Met de versie van agent *06072018* of hoger, kunt u controleren dat zowel de agent en de oplossing zijn geïmplementeerd. U kunt alleen de implementatie van de agent controleren met eerdere versies van de agent.

### <a name="agent-version-06072018-or-later"></a>Agentversie 06072018 of hoger

Voer de volgende opdracht om te controleren dat de agent is geïmplementeerd. 

```
kubectl get ds omsagent --namespace=kube-system
```

De uitvoer moet eruitzien zoals in het volgende, waarmee wordt aangegeven dat deze correct is geïmplementeerd:

```
User@aksuser:~$ kubectl get ds omsagent --namespace=kube-system 
NAME       DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE SELECTOR                 AGE
omsagent   2         2         2         2            2           beta.kubernetes.io/os=linux   1d
```  

Als u wilt controleren of de implementatie van de oplossing, moet u de volgende opdracht uitvoeren:

```
kubectl get deployment omsagent-rs -n=kube-system
```

De uitvoer moet eruitzien zoals in het volgende, waarmee wordt aangegeven dat deze correct is geïmplementeerd:

```
User@aksuser:~$ kubectl get deployment omsagent-rs -n=kube-system 
NAME       DESIRED   CURRENT   UP-TO-DATE   AVAILABLE    AGE
omsagent   1         1         1            1            3h
```

### <a name="agent-version-earlier-than-06072018"></a>Agent-versie ouder is dan 06072018

Om te controleren dat de versie van de Log Analytics-agent vóór uitgebracht *06072018* correct is geïmplementeerd met de volgende opdracht uitvoeren:  

```
kubectl get ds omsagent --namespace=kube-system
```

De uitvoer moet eruitzien zoals in het volgende, waarmee wordt aangegeven dat deze correct is geïmplementeerd:  

```
User@aksuser:~$ kubectl get ds omsagent --namespace=kube-system 
NAME       DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE SELECTOR                 AGE
omsagent   2         2         2         2            2           beta.kubernetes.io/os=linux   1d
```  

## <a name="view-configuration-with-cli"></a>Configuratie met CLI

Gebruik de `aks show` opdracht om informatie te verkrijgen die de oplossing of niet ingeschakeld is, wat is de Log Analytics-werkruimte resourceID en overzichtsgegevens over het cluster.  

```azurecli
az aks show -g <resourceGroupofAKSCluster> -n <nameofAksCluster>
```

Na een paar minuten, de opdracht is voltooid en retourneert JSON opgemaakte informatie over de oplossing.  De resultaten van de opdracht het controle-Add-on-profiel moet worden weergegeven en lijkt op de volgende voorbeelduitvoer:

```
"addonProfiles": {
    "omsagent": {
      "config": {
        "logAnalyticsWorkspaceResourceID": "/subscriptions/<WorkspaceSubscription>/resourceGroups/<DefaultWorkspaceRG>/providers/Microsoft.OperationalInsights/workspaces/<defaultWorkspaceName>"
      },
      "enabled": true
    }
  }
```

## <a name="next-steps"></a>Volgende stappen

* Als u problemen ondervindt tijdens een poging voor de Onboarding van de oplossing, raadpleegt u de [problemen oplossen met](container-insights-troubleshoot.md)

* Als controle is ingeschakeld voor het verzamelen van het status-en resource gebruik van uw AKS-cluster en werk belastingen die erop worden uitgevoerd, leert [u hoe u Azure monitor gebruikt](container-insights-analyze.md) voor containers.


