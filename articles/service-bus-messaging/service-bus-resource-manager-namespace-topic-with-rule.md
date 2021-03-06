---
title: Azure Service Bus-onderwerp en-regel maken met Azure Resource Manager sjabloon | Microsoft Docs
description: Een Service Bus naam ruimte maken met een onderwerp, een abonnement en een regel met behulp van Azure Resource Manager sjabloon
services: service-bus-messaging
documentationcenter: .net
author: spelluru
manager: timlt
editor: ''
ms.assetid: 9e0aaf58-0214-4bca-bd00-d29c08f9b1bc
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 11/27/2019
ms.author: spelluru
ms.openlocfilehash: dc61fb8e066c6189b5607a6e1cd479cb812466f7
ms.sourcegitcommit: c31dbf646682c0f9d731f8df8cfd43d36a041f85
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/27/2019
ms.locfileid: "74561555"
---
# <a name="create-a-service-bus-namespace-with-topic-subscription-and-rule-using-an-azure-resource-manager-template"></a>Een Service Bus naam ruimte maken met een onderwerp, een abonnement en een regel met behulp van een Azure Resource Manager sjabloon

In dit artikel wordt beschreven hoe u een Azure Resource Manager sjabloon gebruikt waarmee een Service Bus naam ruimte wordt gemaakt met een onderwerp, een abonnement en een regel (filter). In het artikel wordt uitgelegd hoe u kunt opgeven welke resources worden geïmplementeerd en hoe para meters moeten worden gedefinieerd die worden opgegeven wanneer de implementatie wordt uitgevoerd. U kunt deze sjabloon gebruiken voor uw eigen implementaties of de sjabloon aanpassen aan uw eisen

Zie [Azure Resource Manager sjablonen ontwerpen][Authoring Azure Resource Manager templates]voor meer informatie over het maken van sjablonen.

Zie [Aanbevolen naamgevings regels voor Azure-resources][Recommended naming conventions for Azure resources]voor meer informatie over de procedures en patronen voor Azure resources-naamgevings conventies.

Voor de volledige sjabloon, zie de [Service Bus naam ruimte met onderwerp, abonnement en regel][Service Bus namespace with topic, subscription, and rule] sjabloon.

> [!NOTE]
> De volgende Azure Resource Manager sjablonen zijn beschikbaar voor downloaden en implementeren.
> 
> * [Een Service Bus naam ruimte maken met een wachtrij-en autorisatie regel](service-bus-resource-manager-namespace-auth-rule.md)
> * [Een Service Bus naam ruimte maken met wachtrij](service-bus-resource-manager-namespace-queue.md)
> * [Een Service Bus naam ruimte maken](service-bus-resource-manager-namespace.md)
> * [Een Service Bus naam ruimte met een onderwerp en een abonnement maken](service-bus-resource-manager-namespace-topic.md)
> 
> Als u de meest recente sjablonen wilt controleren, gaat u naar de galerie met [Azure Quick Start sjablonen][Azure Quickstart Templates] en zoekt u naar Service Bus.
> 
> 

## <a name="what-do-you-deploy"></a>Wat gaat u implementeren?

Met deze sjabloon implementeert u een Service Bus naam ruimte met een onderwerp, een abonnement en een regel (filter).

[Service Bus-onderwerpen en-abonnementen](service-bus-queues-topics-subscriptions.md#topics-and-subscriptions) bieden een een-op-veel communicatie vorm, in een patroon voor *publiceren/abonneren* . Wanneer u onderwerpen en abonnementen gebruikt, communiceren onderdelen van een gedistribueerde toepassing niet rechtstreeks met elkaar. in plaats daarvan wisselen ze berichten uit via een onderwerp dat fungeert als een intermediair. Een abonnement op een onderwerp lijkt op een virtuele wachtrij die kopieën ontvangt van berichten die naar het onderwerp zijn verzonden. Met een filter op het abonnement kunt u opgeven welke berichten die naar een onderwerp worden verzonden, moeten worden weer gegeven in een specifiek onderwerp-abonnement.

## <a name="what-are-rules-filters"></a>Wat zijn regels (filters)?

In veel gevallen moeten berichten met specifieke kenmerken op verschillende manieren worden verwerkt. Als u deze aangepaste verwerking wilt inschakelen, kunt u abonnementen configureren om berichten te vinden die specifieke eigenschappen hebben en vervolgens wijzigingen aan die eigenschappen uit te voeren. Hoewel Service Bus-abonnementen alle berichten zien die naar het onderwerp worden verzonden, kunt u alleen een subset van die berichten kopiëren naar de wachtrij voor virtuele abonnementen. Het wordt uitgevoerd met behulp van abonnements filters. Zie [regels en acties](service-bus-queues-topics-subscriptions.md#rules-and-actions)voor meer informatie over regels (filters).

Klik op de volgende knop om de implementatie automatisch uit te voeren:

[![Implementeren in Azure](./media/service-bus-resource-manager-namespace-topic/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-servicebus-create-topic-subscription-rule%2Fazuredeploy.json)

## <a name="parameters"></a>Parameters

Met Azure Resource Manager definieert u de para meters voor waarden die u wilt opgeven wanneer de sjabloon wordt geïmplementeerd. De sjabloon bevat een sectie met de naam `Parameters` die alle parameterwaarden bevat. Definieer een para meter voor die waarden die variëren op basis van het project dat u implementeert of op basis van de omgeving waarin u implementeert. Definieer geen parameters voor waarden die altijd hetzelfde blijven. De waarde van elke parameter wordt gebruikt in de sjabloon voor het definiëren van de resources die worden geïmplementeerd.

De sjabloon definieert de volgende para meters:

### <a name="servicebusnamespacename"></a>serviceBusNamespaceName
De naam van de Service Bus naam ruimte die moet worden gemaakt.

```json
"serviceBusNamespaceName": {
"type": "string"
}
```

### <a name="servicebustopicname"></a>serviceBusTopicName
De naam van het onderwerp dat in de Service Bus naam ruimte is gemaakt.

```json
"serviceBusTopicName": {
"type": "string"
}
```

### <a name="servicebussubscriptionname"></a>serviceBusSubscriptionName
De naam van het abonnement dat in de naam ruimte van de Service Bus is gemaakt.

```json
"serviceBusSubscriptionName": {
"type": "string"
}
```
### <a name="servicebusrulename"></a>serviceBusRuleName
De naam van de regel (filter) die is gemaakt in de Service Bus naam ruimte.

```json
   "serviceBusRuleName": {
   "type": "string",
  }
```
### <a name="servicebusapiversion"></a>serviceBusApiVersion
De API-versie van de Service Bus van de sjabloon.

```json
"serviceBusApiVersion": { 
       "type": "string", 
       "defaultValue": "2017-04-01", 
       "metadata": { 
           "description": "Service Bus ApiVersion used by the template" 
       }
```
## <a name="resources-to-deploy"></a>Resources om te implementeren
Hiermee maakt u een standaard Service Bus naam ruimte van het type **berichten**, met onderwerp en abonnement en regels.

```json
 "resources": [{
        "apiVersion": "[variables('sbVersion')]",
        "name": "[parameters('serviceBusNamespaceName')]",
        "type": "Microsoft.ServiceBus/Namespaces",
        "location": "[variables('location')]",
        "sku": {
            "name": "Standard",
        },
        "resources": [{
            "apiVersion": "[variables('sbVersion')]",
            "name": "[parameters('serviceBusTopicName')]",
            "type": "Topics",
            "dependsOn": [
                "[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"
            ],
            "properties": {
                "path": "[parameters('serviceBusTopicName')]"
            },
            "resources": [{
                "apiVersion": "[variables('sbVersion')]",
                "name": "[parameters('serviceBusSubscriptionName')]",
                "type": "Subscriptions",
                "dependsOn": [
                    "[parameters('serviceBusTopicName')]"
                ],
                "properties": {},
                "resources": [{
                    "apiVersion": "[variables('sbVersion')]",
                    "name": "[parameters('serviceBusRuleName')]",
                    "type": "Rules",
                    "dependsOn": [
                        "[parameters('serviceBusSubscriptionName')]"
                    ],
                    "properties": {
                        "filterType": "SqlFilter",
                        "sqlFilter": {
                            "sqlExpression": "StoreName = 'Store1'",
                            "requiresPreprocessing": "false"
                        },
                        "action": {
                            "sqlExpression": "set FilterTag = 'true'"
                        }
                    }
                }]
            }]
        }]
    }]
```

Voor de JSON-syntaxis en-eigenschappen raadpleegt u [naam ruimten](/azure/templates/microsoft.servicebus/namespaces), [onderwerpen](/azure/templates/microsoft.servicebus/namespaces/topics), [abonnementen](/azure/templates/microsoft.servicebus/namespaces/topics/subscriptions)en [regels](/azure/templates/microsoft.servicebus/namespaces/topics/subscriptions/rules).

## <a name="commands-to-run-deployment"></a>Opdrachten om implementatie uit te voeren
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a>PowerShell
```powershell
New-AzureResourceGroupDeployment -Name \<deployment-name\> -ResourceGroupName \<resource-group-name\> -TemplateUri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-topic-subscription-rule/azuredeploy.json>
```

## <a name="azure-cli"></a>Azure CLI
```azurecli
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-topic-subscription-rule/azuredeploy.json>
```

## <a name="next-steps"></a>Volgende stappen
Meer informatie over het beheren van deze resources door de volgende artikelen te bekijken:

* [Azure Service Bus beheren](service-bus-management-libraries.md)
* [Service Bus met PowerShell beheren](service-bus-manage-with-ps.md)
* [Service Bus-resources beheren met de Service Bus Explorer](https://github.com/paolosalvatori/ServiceBusExplorer/releases)

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/resource-group-authoring-templates.md
[Azure Quickstart Templates]: https://azure.microsoft.com/documentation/templates/?term=service+bus
[Learn more about Service Bus topics and subscriptions]: service-bus-queues-topics-subscriptions.md
[Using Azure PowerShell with Azure Resource Manager]: ../azure-resource-manager/powershell-azure-resource-manager.md
[Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../azure-resource-manager/xplat-cli-azure-resource-manager.md
[Recommended naming conventions for Azure resources]: /azure/cloud-adoption-framework/ready/azure-best-practices/naming-and-tagging
[Service Bus namespace with topic, subscription, and rule]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-topic-subscription-rule/
[Service Bus queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md

