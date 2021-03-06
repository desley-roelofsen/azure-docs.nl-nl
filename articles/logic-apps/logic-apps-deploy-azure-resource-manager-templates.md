---
title: Sjablonen voor logische app gebruiken
description: Meer informatie over het implementeren van Azure Resource Manager sjablonen die zijn gemaakt voor Azure Logic Apps
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 08/01/2019
ms.openlocfilehash: 73c4c8839bd1d9e37f3dc4f0802ad5a648b1026b
ms.sourcegitcommit: 76b48a22257a2244024f05eb9fe8aa6182daf7e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/03/2019
ms.locfileid: "74793049"
---
# <a name="deploy-azure-resource-manager-templates-for-azure-logic-apps"></a>Azure Resource Manager sjablonen voor Azure Logic Apps implementeren

Nadat u een Azure Resource Manager sjabloon voor uw logische app hebt gemaakt, kunt u uw sjabloon op de volgende manieren implementeren:

* [Azure-portal](#portal)
* [Visual Studio](#visual-studio)
* [Azure PowerShell](#powershell)
* [Azure CLI](#cli)
* [Azure Resource Manager REST API](../azure-resource-manager/resource-group-template-deploy-rest.md)
* [Azure DevOps Azure-pijp lijnen](#azure-pipelines)

<a name="portal"></a>

## <a name="deploy-through-azure-portal"></a>Implementeren via Azure Portal

Als u een sjabloon voor een logische app automatisch wilt implementeren in azure, kunt u de volgende wizard **implementeren naar Azure** kiezen, waarmee u zich aanmeldt bij de Azure Portal en u wordt gevraagd om informatie over uw logische app. U kunt vervolgens alle benodigde wijzigingen aanbrengen in de sjabloon of para meters van de logische app.

[![Implementeren in Azure](./media/logic-apps-deploy-azure-resource-manager-templates/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-logic-app-create%2Fazuredeploy.json)

U wordt bijvoorbeeld gevraagd naar de volgende informatie nadat u zich hebt aangemeld bij de Azure Portal:

* Naam van het Azure-abonnement
* De resource groep die u wilt gebruiken
* Locatie van logische app
* De naam voor uw logische app
* Een test-URI
* Acceptatie van de opgegeven voor waarden

Zie de volgende onderwerpen voor meer informatie:

* [Overzicht: de implementatie voor logische apps met Azure Resource Manager sjablonen automatiseren](logic-apps-azure-resource-manager-templates-overview.md)
* [Resources implementeren met Azure Resource Manager sjablonen en de Azure Portal](../azure-resource-manager/resource-group-template-deploy-portal.md)

<a name="visual-studio"></a>

## <a name="deploy-with-visual-studio"></a>Implementeren met Visual Studio

Als u een sjabloon voor een logische app wilt implementeren vanuit een Azure-resource groeps project dat u hebt gemaakt met behulp van Visual Studio, voert u [de volgende stappen uit om uw logische app hand matig te implementeren](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md#deploy-logic-app-to-azure) in Azure.

<a name="powershell"></a>

## <a name="deploy-with-azure-powershell"></a>Implementeren met Azure PowerShell

Als u wilt implementeren in een specifieke *Azure-resource groep*, gebruikt u de volgende opdracht:

```powershell
New-AzResourceGroupDeployment -ResourceGroupName <Azure-resource-group-name> -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-logic-app-create/azuredeploy.json
```

Zie de volgende onderwerpen voor meer informatie:

* [Resources implementeren met Resource Manager-sjablonen en Azure PowerShell](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy)
* [`New-AzResourceGroupDeployment`](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroupdeployment)

<a name="cli"></a>

## <a name="deploy-with-azure-cli"></a>Implementeren met Azure CLI

Als u wilt implementeren in een specifieke *Azure-resource groep*, gebruikt u de volgende opdracht:

```azurecli
az group deployment create -g <Azure-resource-group-name> --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-logic-app-create/azuredeploy.json
```

Zie de volgende onderwerpen voor meer informatie:

* [Resources implementeren met Resource Manager-sjablonen en Azure CLI](../azure-resource-manager/resource-group-template-deploy-cli.md) 
* [`az group deployment create`](https://docs.microsoft.com/cli/azure/group/deployment?view=azure-cli-latest#az-group-deployment-create)

<a name="azure-pipelines"></a>

## <a name="deploy-with-azure-devops"></a>Implementeren met Azure DevOps

Voor het implementeren van sjablonen voor logische apps en het beheren van omgevingen, gebruiken teams meestal een hulp programma zoals [Azure-pijp lijnen](https://docs.microsoft.com/azure/devops/pipelines/get-started/what-is-azure-pipelines) in [Azure DevOps](https://docs.microsoft.com/azure/devops/user-guide/what-is-azure-devops-services). Azure-pijp lijnen bieden een [implementatie taak voor een Azure-resource groep](https://github.com/Microsoft/azure-pipelines-tasks/tree/master/Tasks/AzureResourceGroupDeploymentV2) die u kunt toevoegen aan een pijp lijn van een build of release. Voor autorisatie om de release pijplijn te implementeren en te genereren, hebt u ook een [Service-Principal](../active-directory/develop/app-objects-and-service-principals.md)voor Azure Active Directory (AD) nodig. Meer informatie over het [gebruik van service-principals met Azure-pijp lijnen](https://docs.microsoft.com/azure/devops/pipelines/library/connect-to-azure).

Zie de volgende onderwerpen en voor beelden voor meer informatie over doorlopende integratie en doorlopende implementatie (CI/CD) voor Azure Resource Manager sjablonen met Azure-pijp lijnen:

* [Resource Manager-sjablonen integreren met Azure-pijp lijnen](../azure-resource-manager/vs-resource-groups-project-devops-pipelines.md)
* [Zelf studie: doorlopende integratie voor Azure Resource Manager sjablonen met Azure-pijp lijnen](../azure-resource-manager/resource-manager-tutorial-use-azure-pipelines.md)
* [Voor beeld: verbinding maken met Azure Service Bus wacht rijen van Azure Logic Apps en implementeren met Azure-pijp lijnen in azure DevOps](https://docs.microsoft.com/samples/azure-samples/azure-logic-apps-deployment-samples/connect-to-azure-service-bus-queues-from-azure-logic-apps-and-deploy-with-azure-devops-pipelines/)
* [Voor beeld: verbinding maken met Azure Storage accounts vanuit Azure Logic Apps en implementeren met Azure-pijp lijnen in azure DevOps](https://docs.microsoft.com/samples/azure-samples/azure-logic-apps-deployment-samples/connect-to-azure-storage-accounts-from-azure-logic-apps-and-deploy-with-azure-devops-pipelines/)
* [Voor beeld: een actie van een functie-app instellen voor Azure Logic Apps en implementeren met Azure-pijp lijnen in azure DevOps](https://docs.microsoft.com/samples/azure-samples/azure-logic-apps-deployment-samples/set-up-an-azure-function-app-action-for-azure-logic-apps-and-deploy-with-azure-devops-pipelines/)
* [Voor beeld: verbinding maken met een integratie account van Azure Logic Apps en implementeren met Azure-pijp lijnen in azure DevOps](https://docs.microsoft.com/samples/azure-samples/azure-logic-apps-deployment-samples/connect-to-an-integration-account-from-azure-logic-apps-and-deploy-by-using-azure-devops-pipelines/)

Hier volgen de algemene stappen op hoog niveau voor het gebruik van Azure-pijp lijnen:

1. Maak een lege pijp lijn in azure-pijp lijnen.

1. Kies de resources die u nodig hebt voor de pijp lijn, zoals de sjabloon voor logische apps en sjabloon parameters, die u hand matig of als onderdeel van het bouw proces genereert.

1. Zoek en voeg de implementatie taak voor de **Azure-resource groep** toe aan de agent taak.

   ![Taak voor de implementatie van een Azure-resource groep toevoegen](./media/logic-apps-deploy-azure-resource-manager-templates/add-azure-resource-group-deployment-task.png)

1. Configureren met een [Service-Principal](https://docs.microsoft.com/azure/devops/pipelines/library/connect-to-azure).

1. Verwijzingen toevoegen aan de sjabloon voor logische apps en sjabloon parameter bestanden.

1. Ga verder met het samen stellen van stappen in het release proces voor alle andere omgevingen, geautomatiseerde tests of goed keurders als dat nodig is.

<a name="authorize-oauth-connections"></a>

## <a name="authorize-oauth-connections"></a>OAuth-verbindingen autoriseren

Na de implementatie werkt uw logische app end-to-end met geldige para meters. U moet echter nog steeds OAuth-verbindingen autoriseren om geldige toegangs tokens te genereren voor [het verifiëren van uw referenties](../active-directory/develop/authentication-scenarios.md). Hier volgen manieren waarop u OAuth-verbindingen kunt autoriseren:

* Voor automatische implementaties kunt u een script gebruiken dat toestemming geeft voor elke OAuth-verbinding. Hier volgt een voor beeld van een script in GitHub in het [LogicAppConnectionAuth](https://github.com/logicappsio/LogicAppConnectionAuth) -project.

* Als u OAuth-verbindingen hand matig wilt autoriseren, opent u de logische app in de ontwerp functie voor logische apps, in de Azure Portal of in Visual Studio. Machtig de vereiste verbindingen in de ontwerp functie.

Als u een [Service-Principal](../active-directory/develop/app-objects-and-service-principals.md) van Azure Active Directory (Azure AD) gebruikt om verbindingen te autoriseren, leert u hoe u [para meters voor de Service-Principal opgeeft in uw sjabloon voor logische apps](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md#authenticate-connections).

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Logische apps controleren](../logic-apps/logic-apps-monitor-your-logic-apps.md)
